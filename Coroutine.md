<!-- https://www.cnblogs.com/Dearmyh/p/9317699.html -->
<!-- https://zhuanlan.zhihu.com/p/59619632 -->
<!-- https://blog.csdn.net/huang9012/article/details/38492937 -->

> ### 一些概念理解

协程：协同程序，在主线程运行同时，开启另一段逻辑处理，来协同当前程序的执行。在Unity中协程与其他语言有一定差异，可以理解为: 注册一个每帧都可以继续执行的函数，且每次都到yield处暂停。

Coroutine来自Monobehaviour，为Unity中概念，而非C#，但基于C# 一个强大的接口IEnumerator，他允许你为自己的集合类型编写枚举器。

Unity协程暂停后会立即返回主函数，执行主函数剩余部分，直到中断指令完成后，从中断指令的下一行继续执行协程剩余的函数，函数体全部执行完成，协程结束。

性能上相对于一般函数没更多额外开销。

好处：让原本用异步+回调方式写的非人类代码，可以看成看似同步的方式解决。能分步做一个耗时的任务，分散计算压力。

坏处：协程基于迭代器，且基于Unity生命周期，大量开启协程会引起GC。同时激活的协程太多还可能出现多个高开销的协程挤在同一帧执行，导致卡帧。

协程书写的性能优化：常见的问题是直接new一个终端指令，带来不必要的GC负担，可以服用一个全局的中断指令对象，优化开销；在Yielders.cs这个文件里，已经集中地创建了上面这行类型的静态对象。[参见一篇分析文章](https://blog.csdn.net/liujunjie612/article/details/70623943)

协程不是线程，不是异步执行，而是和Monobehaviour的updata函数一样也是在主线程执行。Unity在每一帧都会处理对象上的协程，也就是协程跟update一样都是Unity每帧都回去处理的函数，一般协程是每帧lateUpdate后运行。

> ### 协程开启方式

1. StartCoroutine(string methodName)

参数是方法名字符串，此方法可以包含一个参数。
形参方法可以有返回值。

2. StartCoroutine(IEnumerator method)

参数是方法体调用(TestMethod())，方法中可以包含多个参数。  
IEnumerator类型的方法不能含有ref或out类型的参数，但可以含有被传递的引用。  
必须有返回值，且返回值类型为IEnumerator，返回值使用 yield return 表达式(或值) 或 yield break 语句

> ### 协程终止方式

1. 函数名字符串开启与终止

```
StartCoroutine("Test1"); // 开启
StopCoroutine("Test1"); // 停止
```

2. 接收开启时返回值然后利用该值终止

```
private Coroutine coroutine2;

coroutine2 = StartCoroutine(Test2()); // 开启并存储返回值

StopCoroutine(coroutine2); // 停止
```

3. StopAllCoroutine()

终止当前脚本下所有协程。

**注意**：慎重使用，因为可能后续修改逻辑时新建协程被意外终止， 需要确定调用脚本的全部协程都需要被终止(如断线重连重置所有状态时)

4. 禁用或销毁对象

gameObject.active = false 可以停止该对象上全部协程的执行，即使再次激活也不能继续执行。但注意Monobehaviour enable = false不能停止协程，对比update却可以在Monobehaviour enable = false就终止。这是因为协程StartCoroutine时被注册到GameObject上，他的声明周期受限于GameObject生命周期，所以受到GameObject 是否active影响。但协程虽然是在Monobehaviour启动的StartCoroutine，但是协程函数的地位却跟Monobehaviour一个层次，所以不受Monobehaviour影响。


> ### 协程的执行原理

协程函数的返回值是IEnumerator迭代器，可以把它当成执行一个序列的某个节点的指针。他提供了三个基本操作(接口)，分别是Current 返回当前指向的元素，MoveNext() 将指针往后移动一个单位，返回结果true/false, Reset 重置到第一个元素前位置。

> ### 协程结束的标志

如果最后一个yield return 的 IEnumerator 已经迭代到最后一个时，MoveNext就会返回false，这时Unity就会将这个IEnumerator从coroutines list中移除。只有当这个对象的MoveNext()返回false时，即IEnumerator中的Current已经迭代到最后一个元素了才会执行 yield return 后面的语句

> ### 中断函数类型

yield: 挂起，程序遇到yield关键字就会被挂起，暂停执行，等条件满足时从当前位置继续执行

yield return null / yield return 0 在下一帧所有Update()函数调用之后从当前位置继续执行。   

yield return new WaitForSeconds(n)等待指定秒数，在该帧(延迟过后的那一帧)所有Update()函数调用完后执行。即等待给定时间周期，受Time.timeScale影响，当Time.timeScale = 0f时，yield return new WaitForSecond(x)将不满足。   

yield new WaitForFixedUpdate() 等待一个固定帧，即等待物理周期循环结束后执行(所有脚本中的FixedUpdate()函数都被执行)后从当前位置继续执行。   

yield new WaitForEndOfFrame() 等待帧结束(所有的渲染及GUI程序执行完)，即等待渲染周期循环结束后执行。   

yield return StartCoroutine() 等待一个新协程完成后从当前位置继续执行

> ### 协程的执行顺序

开始协程 -> 执行协程 -> 遇到中断指令中断协程 -> 返回上层函数继续执行上层函数的下一行代码 -> 中断指令结束后继续执行中断指令后的代码 -> 协程结束

> ### 协程嵌套

协程可以嵌套，yield return StartCoroutine 就是。执行顺序是：子协程中断后，会返回父协程，父协程暂停，返回父协程的上一级函数。
决定父协程结束的标志是子协程是否结束，当子协程结束后返回父协程执行其后的代码才算结束。


在一个MonoBehaviour提供的主线程里同一时刻只能有一个运行状态的协程。因为协程不是线程，不是并行的，同一时刻，同一脚本实例中可以有多个暂停的协程，但只有一个运行着的协程。




![仅供参考](https://gitee.com/rainopen/images/raw/master/Coroutine.png)

* [Unity 协程(Coroutine)原理与用法详解](https://www.cnblogs.com/xinzhilinger/p/14718527.html)
* [协程原理与用法详解](https://www.cnblogs.com/xinzhilinger/p/14718527.html)


