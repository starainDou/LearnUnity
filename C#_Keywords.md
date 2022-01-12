> ### abstract

抽象关键字，可以和类、方法、属性、索引器、事件一起使用，标识可以扩展不可实体化且必须实现

> ### as

转换操作符，转换失败则返回null

> ### base

用于访问被派生类或构造中的同名成员隐藏的基类成员

> ### catch

定义一个代码块，在特定类型异常抛出时执行块内代码

> ### checked

既是操作符又是语句，确保编译器运行时，检查整数类型操作或转换时出现的溢出

> ### const

标识一个可在编译时计算出来的变量值，一经指派不可修改

> ### delegate

指定一个声明为一种委托类型，委托把方法封装为可调用的实体，能在委托实体中调用

> ### enum

表示一个已命名常量群体的值类型(枚举)

> ### event

允许一个类或对象提供通知的成员，他必须是委托类型

> ### explicit

一个定义用户自定义转换操作符的操作符，通常用来将内建类型转换为用户定义类型或反向操作，必须在转换时调用显示转换操作符

> ### extern

标识一个将在外部(通常不为C#语言)实现的方法

> ### finally

定义一个代码块，在程序控制离开try代码后执行

> ### fixed

在一个代码块执行时，在固定内存位置为一个变量指派一个指针

> ### foreach

用于遍历一个群集的元素

> ### goto

跳转语句，将程序执行重定向到一个标签语句

> ### Partial

局部关键字
https://www.cnblogs.com/bincoding/p/7337257.html


> ### sealed

1. 当 sealed 修饰类时，会阻止其他类继承此类，等同于其他语言 final。
2. 当 sealed 修饰方法或属性时，防止子类重写该方法或属性

sealed 修饰虚方法或虚属性，可同 override一起使用，但如果不是虚方法或虚属性则报错误 cannot be sealed because it is not an override

sealed 防止子类重写特定方法或属性

```
public class A
{
	protected virtual void M() { Console.WriteLine("A.M()");}
	protected virtual void M1() { Console.WriteLine("A.M1()");}
}

public class B: A
{
	protected sealed override void M() { Console.WriteLine("B.M()");}
	protected override void M1() { Console.WriteLine("B.M1()"); }
}

public sealed class C: B
{
	protected override void M1() { Console.WriteLine("C.M1()");}
	// protected override void M() { Console.WriteLine("C.M()");}
	// 报错 cannot override inherited member 'ConsoleApplication1.MSFun.Sealed.B.M()' 
}
```

> ### using

1. using + 命名空间：类似引入命名空间内容，就不用再指定命名空间就可以用其中的类型。   
2. using + 别名 = 命名空间.具体类型：当两个不同命名空间出现同名类时就很好用

```
using aClass = Namespace1.MyClass
using bClass = Namespcae2.MyClass
```

3. using 指定范围

场景：当在某个代码中使用了类的实例，希望只要离开这个代码段就调用类实例的Dispose。
使用try...catch来捕获异常也可，但用using更方便

```
using (MyClass cls1 = new MyClass(), cls2 = new MyClass())
{
	// the code using cls1, cls2
	// call the Dispose on cls1 and cls2
}
```

> ### virtual

一般函数在编译时就静态地编译到了执行文件中，其相对地址在程序运行期间是不发生变化的。   
而虚函数在编译期间不被静态编译，他的相对地址是不确定的，他会根据运行时对象实例来判断要调用的函数。   
其中那个申明时定义的类叫做申明类，那个执行时实例化的类叫做实例类。   

检查流程   
1. 当调用一个对象的函数时，系统会直接检查这个对象申明定义的类，即申明类，看所调用的函数是否为此虚函数。   
2. 


* [官方关键字文档](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/keywords/)   
* [explicit和implicit](https://www.cnblogs.com/yilezhu/p/10898582.html)   
* [C#关键字大全](https://www.sohu.com/a/117719144_467799)   
* [虚方法virtual详解](https://www.cnblogs.com/zhaoshujie/p/10502404.html)
* [C#中的Partial](https://www.cnblogs.com/bincoding/p/7337257.html)


 