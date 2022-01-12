# Extension

扩展方法能够向现有类型添加方法而无需创建新的派生类、重新编译或其他方式修改原始类型。扩展方法是一种特殊的静态方法，但可以像扩展类型上的实例方法一样调用

```
// 扩展方法
public static class MethodExtension // 用static修饰
{
    public static string myAdd(this string strObj, string addStr) // 用static修饰，第一个参数为调用对象，this修饰
    {
        return String.Format("{0}{1}", strObj, addStr);
    }
}

// 使用
Debug.Log("扩展测试:{0}", "原始字符串+".myAdd("加入的字符串"));
```

1、定义一个静态类以包含扩展方法。该类必须对客户端代码可见。    
2、将该扩展方法实现为静态方法，并使其至少具有与包含类相同的可见性。   
3、该方法的第一个参数指定方法所操作的类型；该参数必须以 this 修饰符开头。   
4、在调用代码中，添加一条 using 指令以指定包含扩展方法类的命名空间。   
5、按照与调用类型上的实例方法一样的方式调用扩展方法   

<!-- https://www.cnblogs.com/lxblog/archive/2013/04/25/3041826.html -->