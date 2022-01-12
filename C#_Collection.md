# Collection 集合

Collection(集合)类是专门用于数据存储和检索的类，这些类提供了对stack(栈)，queue(队列)，list(列表)，hashTable(哈希表)的支持。

> ### ArrayList 动态数组

代表了可被单独索引的对象的有序集合，基本上可以取代数组，与数组不同的是，可以使用索引在指定的位置添加或移除条目，动态改变大小。他也允许在列表中进行动态内存分配、增加、搜索、排序各项。

> ### Hashtable 哈希表

它使用键来访问集合中的元素，哈希表中每一项都有一个键值对

常用属性

```
Count 获取Hashtable中包含的键值对个数
IsFixedSize 获取Hashtable是否具有固定大小
IsReadOnly 获取Hashtable是否只读
Item 获取或者设置指定的键的值
Keys 获取一个ICollection，包含Hashtable中的键
Values 获取一个ICollection，包含Hashtable中的值
```

常用方法

```
// 向Hashtable天津一个带有指定的键值对元素
public virtual void Add(object key, object vlaue);
// 移除Hashtable中所有的元素
public virtual void Clear();
// 判断Hashtable是否包含指定的键
public virtual bool ContainsKey(object key);
// 判断Hashtable是否包含指定的值
public virtual bool ContainsValue(object value);
// 从Hashtable中移除指定的键的元素
public virtual void Remove(object key);
```

示例

```
Hashtable ht = new Hashtable();

ht.Add("01", "LiBai");
ht.Add("02", "DuFu");
ht.Add("03", "LiHe");
if (ht.ContainsValue("LiBai"))
{
	Console.WriteLine("True");
}
ICollection keyArray = ht.Keys;
foreach(string k in keyArray)
{
	Console.WriteLine("key:{0}, value:{1}", k, ht[k]);
}
Console.ReadKey();
```

> ### SortedList 排序列表

他可以使用键和索引来访问表中的项，是数组和哈希表的组合，他包含一个可以使用键或索引访问各项的列表，如果使用索引访问各项，则他是一个动态数组ArrayList，如果使用键来访问各项，则他是一个哈希表Hashtable。集合中的各项总是按键值排序。

> ### Stack 栈

他是一个先进后出的对象集合，当需要对各项进行后进先出访问时则使用栈。当添加一项时称为推入元素，当从列表中移除一项时称为弹出元素。

> ### Queue 队列

他代表一个先进先出的对象集合。当需要对各项进行先进先出访问时则使用队列。当添加一项时称为入队，当从列表中移除一项时称为出队。

> ### BitArray 点列阵

他代表了使用值0和1来表示的二进制数组。当需要存储位，但事先不知道位数时，则使用点列阵。可使用整数索引从点列阵集合中访问各项，索引从0开始

常用属性

```
Count 获取BitArray中包含的元素个数
IsReadOnly BitArray是否只读
Item 获取或设置BitArray中指定位置的位的值
Length 获取或设置BitArray中的元素个数
```

常用方法

```
// 对当前BitArray中的元素和指定的BitArray中的相对应的元素执行按位与操作
public BitArray And(BitArray value);
// 获取BitArray中指定位置的位的值
public bool Get(int index);
// 把当前的BitArray中的值反转，true -> false，false -> true
public BitArray Not();
// 把当前的BitArray中的元素和指定的BitArray中相对应的元素执行按位或操作
public BitArray Or(BitArray value);
// 把BitArray中指定位置的位设置为指定的值
public void Set(int index, bool value);
// 把BitArray中所有位设置为指定的值
public void SetAll(bool value);
// 对当前的BitArray中的元素和指定的BitArray中的相对应的元素执行按位异或操作
public BitArray Xor(BitArray value);
```

> ### List 泛型集合

##### 为什么要用泛型集合？

C# 2.0 之前主要通过两种方式实现集合

1. ArrayList

直接将对象放入ArrayList，操作直观，但由于集合中的项是Object类型，每次使用都必须经过繁琐的类型转换，效率低下

2. 使用自定义集合类

比较常见的做法是从CollectionBase抽象类继承一个自定义类，通过对IList对象进行封装实现强类型集合。这种方式要求为每种集合写一个相应的自定义类，工作量较大。

此时泛型集合的出现很好的解决了上述问题

##### List<T>泛型集合使用示例

```
class Person
{
	private string _name;
	private int _age
	
	public Person(string name, int age)
	{
		this._name = name;
		this._age = age;
	}
	
	public string Name 
	{
		get { return _name; }
	}
	
	public string Age
	{
		get => _age;
	}
}

Person person1 = new Person("LiBai", 18);
Person person2 = new Person("DuFu", 17);

List<Person> personList = new List<Person>();
personList.Add(person1);
personList.Add(person2);

Console.WriteLine(person[1].Name);
```

##### 泛型集合排序

排序基于比较

<!-- https://www.cnblogs.com/dyhao/p/9501479.html -->

* [数组](https://blog.csdn.net/Memoryuuu/article/details/53425289)
* [List和dictionary集合解析](https://blog.csdn.net/jjl1991_11/article/details/80135218)