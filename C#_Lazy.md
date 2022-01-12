# Lazy

从 .net 4.0 开始，C#开始支持延迟初始化，通过Lazy关键字加泛型，声明某个对象在第一次使用时才初始化，如果一直没有用就不初始化，提升效率，同时Lazy天生线程安全

```
class MyClass
{
    // 成员变量
    public string name { get; private set; }
    // 构造函数是一个特殊的成员函数，名称和类名相同，无任何返回类型，创建类时调用
    public MyClass()
    {
        Console.WriteLine("MyClass 默认构造函数");
    }

    public MyClass(string inputName)
    {
        name = inputName;
        Console.WriteLine("MyClass 自定义构造函数参数:{0}", name);
    }
}
```

##### 构造时使用默认初始化方式

```
Lazy<MyClass> myClass3 = new Lazy<MyClass>();
if(myClass3.IsValueCreated)
{
    Console.WriteLine("已经初始化");
}
else
{
    Console.WriteLine("没有初始化");
}
Console.WriteLine("这是第二次，所以已经初始化了");
Console.readLine();
```

##### 构造时使用指定的委托初始化

```
Lazy<MyClass> myClass4 = new Lazy<MyClass>(
    () =>
    {
        return new MyClass(inputName: "懒加载输入的名字");
    });
if(myClass4.IsValueCreated)
{
    Console.WriteLine("已经初始化");
}
else
{
    Console.WriteLine("没有初始化");
}
Console.WriteLine("这是第二次，所以已经初始化了, name = {0}", myClass4.Value.name);
```

##### Lazy.Value 

Lazy 对象创建后，并不会立即创建对应对象，只有在变量Value属性在首次访问时才会真正创建，同时将其缓存到Value中，以便将来访问。   
Value属性是只读的，也就意味着如果Value存储了引用类型，将无法为其分配新对象，只可以更改此对象公共的属性或字段等，如果Value存储的是值类型，就不能修改其值了，只能通过再次调用变量的函数使用新的参数来创建新的变量。

[参考](https://blog.csdn.net/mingtianqingtian/article/details/112174792)