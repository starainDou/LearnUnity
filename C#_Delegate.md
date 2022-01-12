> #### Delegate 委托

C#中的委托类似于C/C++中的函数指针，但完全面向对象，实质是类，派生自System.Delegate类，属于引用类型。表示对具有特定参数列表和返回类型的方法的引用。

声明委托：<Access Specifier> delegate <Return Type> <Delegate Name>(Parameter List)

```
using System;

namespace DelegateTest
{
    public class TestDelegate
    {
        public delegate int HandleDelegate(int num);

        private static int originalNum = 3; 
        public static int AddNumer(int num)
        {
            originalNum += num;
            return originalNum;
        }

        public static int MultNumber(int num)
        {
            originalNum *= num;
            return originalNum;
        }

        public static void Test()
        {
            HandleDelegate handle1 = new HandleDelegate(AddNumer);
            HandleDelegate handle2 = new HandleDelegate(MultNumber);
            int result1 = handle1.Invoke(5);
            int result2 = handle2.Invoke(7);
            Console.WriteLine("result1:{0} result2:{1}", result1, result2);
        }
    }
}
```

> ### 委托的多播

```
public static void MulticastingOfDelegateTest()
{
    // C# 1.0
    HandleDelegate handle1 = new HandleDelegate(AddNumer);
    // C# 2.0 后
    HandleDelegate handle2 = MultNumber;
    // C# 2.0 后可以匿名函数
    HandleDelegate handle3 = delegate(int num)
    {
        originalNum += num;
        return originalNum;
    };
    // C# 3.0 后可以 lambda表达式
    HandleDelegate handle4 = num =>
    {
        originalNum += num;
        return originalNum;
    };
    HandleDelegate handleMulticating = handle1 + handle2 + handle3;
    handleMulticating += handle4;
    var result1 = handle1(5);
    var result2 = handle2(7);
    var result3 = handle3(1);
    var result4 = handle4(2);
    Console.WriteLine("result1:{0} result2:{1} result3:{2} result4:{3}", result1, result2, result3, result4);
}
```

> ### 匿名方法

匿名方法提供了一种传递代码块作为委托参数的技术，匿名方法是没有名称只有主体的方法。   
在匿名方法中不需要指定返回类型，他是从方法主体内的return语句推断的。

```
public delegate void NumberChanger(int n); // 委托

NumberChanger nc = delegate(int x)
{
	Console.WriteLine("Param: {0}", x);
}
```

> ### Lambda表达式

Lambda表达式实际是一种匿名函数，在Lambda表达式中可以包含语句以及运算等操作，可用于创建委托或表达式目录树类型，支持带有可绑定到委托或表达式树的输入参数的内联表达式，使用Lambda表达式可大大减少代码量，使得代码更加简洁，优美。


Lambda运算符 => 读作 goes to


> ### Action


> ### 