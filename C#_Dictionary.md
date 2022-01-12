# Dictionary

必须包含名空间System.Collection.Generic   
Dictionay 里的每个元素都是一个键值对，Key唯一，Value不一定唯一   
Key和Value可以是任何类型   

定义

```
Dictionary<string, string> tempDict = new Dictionary<string, string>();
```

添加元素

```
tempDict.Add("1", "赵");
tempDict.Add("2", "钱");
tempDict.Add("3", "孙");
tempDict.Add("4", "李");

// 添加已经存在的Key会抛异常
try
{
    tempDict.Add("4", "李2");
}
catch (Exception e)
{
    Console.WriteLine("Add添加的Key重复会抛异常{0}", e.Message);
}
```

修改元素

```
tempDict["1"] = "赵1";
tempDict["2"] = null;
```

取值

```
var value = tempDict["1"]
Console.WriteLine("{0}", value);
```

删除

```
tempDict.Remove("3");
```

判断存在性

```
tempDict.ContainsKey("10086")
```

<!-- https://www.cnblogs.com/txw1958/archive/2012/11/07/csharp-dictionary.html -->
https://www.cnblogs.com/vickylinj/p/10922139.html

http://www.u3d8.com/?author=1&paged=20