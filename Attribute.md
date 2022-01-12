> ### Attribute(特性)

公共语言运行时允许添加类似关键字的描述声明，叫做Attribute。  

C# 中存在一定数量的固定的编译器指令，如 #define DEBUG, #undefine DEBUG, #if   
Attibute可以添加更多编译器指令

> ### 附着目标

public enum AttributeTargets

其适用范围为 
* 模块(Assembly) [枚举值 1]
* 模块(Module) [枚举值 2]
* 类型(Class) [枚举值 4] 
* 结构体(Struct) [枚举值 8] 
* 枚举(Enum) [枚举值 16]
* 构造方法(Constructor) [枚举值 32] 
* 实例方法(Method) [枚举值 64]
* 属性(Property) [枚举值 128] 
* 字段(Field) [枚举值 256] 
* 事件(Event) [枚举值 512]
* 接口(Interface) [枚举值 1024]
* 参数(Parameter) [枚举值 2048]
* 委托(Delegate) [枚举值 4096]
* 返回值(ReturnValue) [枚举值 8192]
* 泛型参数(GenericParameter) [枚举值 16384]
* All [枚举值 32767]

可以通过 Convert.ToInt32(AttributeTargets.Assembly) 方式获取其枚举整数值

> ### 系统内置提供的一些Attribute

* Conditional: 起到条件编译作用，满足条件才允许编译器对他的代码进行编译，可用于调试程序

```
#define LiBai
using System;
...

[Conditional("LiBai")]
private static void DisplayRunningMessage() 
只要满足定义了 LiBai 时才会编译，可用于区分debug和release
``` 

* DllImport: 用来编辑非 .Net的函数，表明该方法在一个外部的dll中定义

```
[DllImport("UserMessage.dll")]
public static extern int MessageBox(string msg, string toId);
表明MessageBox是UserMessage.dll中的函数，这样我们可以像内部方法一样调用这个函数
```

* Obsolete: 标记当前方法已经被废弃

```
[Obsolete]
private static void DisplayDebugMessage()
标识 DisplayDebugMessage 已经被废弃
还可以添加信息，[Obsolete("我是一个被废弃的方法", true)]
```

* [Range(0, 10)] 限制数值范围

```

```

* HideInInspector 在Inspector面板中隐藏

共有的变量默认在Inpector面板是可见的，如果不想在面板中可见，则可用HideInInspector隐藏

```
[HideInInspector]
public int Age;
```

* [NonSerialized] 不序列化一个变量，并且在Inspector隐藏

* [SerializeField] 序列化字段[可让private字段在Inspector可见]

Unity中提供了非常方便的功能可以帮助用户将成员变量在Inspector中显示，并且定义Serialize关系。   
也就是凡是在Inspector中的属性都同时具有Serialize功能(序列化的意思是再次读取Unity时序列化的变量时有值的，不需要你再次去赋值，因为他已经被保存下来了)

public 变量
	在没有加入任何Attribute的前提下，public变量默认被视为可以Serialize的，所以一般public的变量在Inspector面板时可见的。而private变量在Inspector面板默认是不可见的。
	
[SerializeField] Attribute
	强制Unity序列化一个私有域，这是Unity内部的序列化功能，有时候我们需要Serialize一个private 或 protected的属性，这时候可以用[SerializeField]这个特性，之后在面板就可以看到该变量了。
	
```
[Range(0, 10)]
[SerializeField]
private float m_SteerHelper;
```

拓展
1. 隐藏共有的序列化变量

```
[HideInInspector]
public int Age;
// Age此时可在代码中赋值，但不会在面板中看到，也就不能手动设置赋值
```

2. 私有的序列化变量想在面板中读取并赋值

```
[SerializeField]
private string FirstName;
```

3. 私有化变量想在面板中看到，但不允许手动赋值

```
[HideInInspector]
[SerializeField]
private Int Score;
public Int ShowScore;
{
	get { return Score; }
}
然后在Editor中显示 EditorGUILayout.LabelField("value". game. ShowScore.ToString());
```

[Unity之SerializeField（序列化字段）和Serializable的一点理解](https://blog.csdn.net/Momo_Da/article/details/71427924)   	
[Unity3D中[SerializeField]特性的使用](https://blog.csdn.net/yangyong0717/article/details/71512251/)

* [Multiline(5)] 字符串最大显示行数

[Multiline(5)] 表示在Inspector面板显示高度为5行，但是单段仍只占一行，多段占多行。

[【Unity3D日常开发】String如何多行输入](https://blog.csdn.net/q764424567/article/details/108049944)

* [FormerlySerializedAs("Value1")]

当变量名发生改变时，可以保存原来Value1 的值

* [ContextMenu("TestButton")]

组件右键菜单按钮

* [Space]

可以与上面的显示内容形成一定的空隙，[Space(25)]

* [Header(“XXX”)]

在Inspector面板上显示标题，具有分组效果

* [Tooltip(“XXX”)]

在Inspector面板上鼠标移上定义的字段弹出描述

* [RequireComponent]

就会在Class的GameObject上自动追加所需的Component

> ### 自定义Attribute


* [Unity-SetTag](https://blog.csdn.net/zhanxxiao/article/details/106120872)
[参考](https://www.cnblogs.com/luckboy/archive/2009/07/18/1526083.html)

[Unity截图](https://blog.csdn.net/mingtianqingtian/article/details/109727245)   
[人物跳跃和相机跟随](https://www.bilibili.com/video/av8919490?from=search&seid=1725606209116286885)
<!-- https://www.kancloud.cn/a173512/unity/1359205 -->
<!-- 下标n<sub>3</sub> 上标n<sup>3</sup> -->
<!-- typeof(...).GetProperty(...).GetCustomAttributes(...) -->
[C#中的Attribute详解(下)](https://blog.csdn.net/xiaouncle/article/details/70229119)
[C#属性(Attribute)用法实例解析](https://www.jb51.net/article/54100.htm)
[Unity基础篇：Serializable总结与深入研究](https://blog.csdn.net/fancybit/article/details/107438976)
[从Unity中的Attribute到AOP](https://www.bbsmax.com/A/amd0ERNjzg/)