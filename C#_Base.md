# C# 基础

> ### 数据类型

##### 值类型 Value Type

值类型变量可以直接分配给一个值，从System.ValueType中派生

1. 整数类型 [8位=1字节]

```
sbyte: 1字节 8位有符号整数类型(short byte) -128 ~ 127，默认0
byte: 1字节 8位无符号整型 0 ~ 255 默认0
short: 2字节 16位有符号整数类型 -32768 ~ 32767 默认0
ushort: 2字节 16位无符号整数类型 0 ~ 65535 默认0
int: 4字节 32位有符号整型 -2147483648 ~ 2147483647 默认0
uint: 4字节 32位无符号整数类型 0 ~ 4,294,967,295 默认0
long: 8字节 64位有符号整数类型 -9,223,372,036,854,775,808 ~ 9,223,372,036,854,775,807 默认0L
ulong: 8字节 64位无符号整数类型 0 ~ 18,446,744,073,709,551,615 默认0
```

2. 字符类型 

```
char: 存储单个Unicode字符，2字节

char firstChar = 'A'; // 用单引号
char[] separator = { '@', '^', ','}; // 字符数组
Console.WriteLine(firstChar);
Console.WriteLine(separator[0]);
```

3. 浮点类型

```
float: 4字节 32位单精度浮点型，精确到7位数 0.0F
double: 8字节 64位双精度浮点型，精确到15~16位 0.0D
decimal: 16字节 128位精确的十进制，精确到28~29位有效数位 0.0M
```

4. 布尔类型

```
bool True / False 默认False 1字节
```

5. 枚举类型

```
enum MessageType {
	enterGame,
	leaveGame,
	pauseGame,
	resumeGame
}
Console.WriteLine("枚举转字符串:{0}", MessageType.enterGame.ToString());
Console.WriteLine("枚举转整型:{0}", (int)MessageType. leaveGame); // 强制转换
Console.WriteLine("整型转枚举方案一:{0}", (MessageType)3); // 强制转换
Console.WriteLine("整型转枚举方案二:{0}", Enum.GetName(typeof(MessageType), 1));
Console.WriteLine("枚举数值字符串转整型:{0}", (MessageType)Enum.Parse(typeof(MessageType), "1"));
Console.WriteLine("枚举名称字符串转整型:{0}", (MessageType)Enum.Parse(typeof(MessageType), "resumeGame"));
// https://blog.csdn.net/qq_45096273/article/details/106618543
```

6. 结构体类型

结构体可带有方法、字段、索引、属性、运算符方法和事件    
结构体可定义带参构造函数，但不能定义析构函数，也不能定义无参构造函数[默认自动定义的]   
结构体不能继承和被继承  
结构体可实现一个或者多个接口   
结构体成员不能指定 abstract、virtual、protected   
使用New操作符创建一个结构体对象时，会调用适当的构造函数。
与类不同，结构体可以不使用New操作符也能实例化
如果不适用New操作符，只有所有字段都被初始化，字段才被赋值，对象才能使用

类 VS 结构体

类是引用类型，结构体是值类型
结构体不能继承和被继承
结构体不能声明默认的构造函数
结构体中声明的字段不能赋初值，类可以
结构体自定义构造函数中必须为所有字段赋值，类构造函数无此限制

```
struct Books
{
	public string title;
	public int book_id;
};

Books book1;
book1.title = "C# Programming";
book1.book_id = 1008611;

Console.WriteLine("title:{0}", book1.title);
Console.WriteLine("bookId:{0}", book1.book_id);
Console.ReadKey();
```



##### 引用类型 Reference Type

1. 对象类型 Object

Object 类型是C#通用类型系统CTS(Common Type System)中所有数据类型的基类，是System.Object类的别名，所以Object类型可以被分配任何其他类型(值类型、引用类型、预定义类型、自定义类型)的值，但是分配值之前要进行类型转换    

当一个值类型转换为对象类型时，则被称为 装箱；另一方面，当一个对象类型转换为值类型时，则被称为 拆箱


2. 动态类型 Dynamic

可以存储任意类型的值在动态数据类型的变量里，类型检查发生在运行时   

```
dynamic tempParam = 20;
```

3. 字符串类型 String

String类型是System.String 类的别名，从Object类型派生

```
// 值的分配方式1
string str1 = "D:\\myDir";
// 值的分配方式2 @ 逐字字符串 转移字符\被当做普通字符
string str2 = @"D:\myDir";
// @字符串可以换行，换行符和缩进空格等都计算在字符串长度内
string str3 = @"<script type=""text/javascript"">
	<!--  -->
</script>";

单行字符串中打印\和" 需要用\\和\" 
逐行字符串中打印\直接打印即可，打印" 要用""
```

##### 用户自定义类型

class、interface、delegate

##### 指针类型 Pointer types

指针类型变量存储另一种类型的内存地址，C#中的指针和C/C++ 中的指针有相同功能

```
// type* identifier
char* cptr
int* iptr
```

##### 类型转换

* 隐式类型转换  以安全方式进行的转换, 不会导致数据丢失。例如，从小的整数类型转换为大的整数类型，从派生类转换为基类。   
* 显式类型转换  即强制类型转换。显式转换需要强制转换运算符，而且强制转换可能会造成数据丢失。

C#内置的一些类型转换方法

```
ToBoolean: 如果可能的话，把类型转换为布尔型   
ToByte: 把类型转换为字节类型
ToChar: 如果可能的话，把类型转换为单个 Unicode 字符类型   
ToDateTime: 把类型（整数或字符串类型）转换为 日期-时间 结构   
ToDecimal: 把浮点型或整数类型转换为十进制类型   
ToDouble: 把类型转换为双精度浮点型    
ToInt16: 把类型转换为 16 位整数类型   
ToInt32: 把类型转换为 32 位整数类型   
ToInt64: 把类型转换为 64 位整数类型   
ToSbyte: 把类型转换为有符号字节类型   
ToSingle: 把类型转换为小浮点数类型   
ToString: 把类型转换为字符串类型   
ToType: 把类型转换为指定类型 
```

> ### 变量和常量

##### 变量

定义语法 <data_type> <variable_list>;

```
// 声明 与 赋值
int i, j;
i = 10;
j = 20;
string firstName = "Dianyu", lastName = "Dou";

// 静态变量
class Person {
	static int Age = 6
}

// 成员变量对外只读
public string FirstName { get; private set; }
```

##### 常量

整数常量：0x 或 0X 表示十六进制，0 表示八进制，没有前缀则表示十进制   
整数常量也可以有后缀，可以是 U 和 L 的组合，其中，U 和 L 分别表示 unsigned 和 long。后缀可以是大写或者小写，多个后缀以任意顺序进行组合   

```
85         /* 十进制 */
0213       /* 八进制 */
0x4b       /* 十六进制 */
30         /* int */
30u        /* 无符号 int */
30l        /* long */
30ul       /* 无符号 long */

078        /* 非法：8 不是一个八进制数字 */
032UU      /* 非法：不能重复后缀 */
```

浮点常量：一个浮点常量是由整数部分、小数点、小数部分和指数部分组成。您可以使用小数形式或者指数形式来表示浮点常量

```
3.14159       /* 合法 */
314159E-5L    /* 合法 */

510E          /* 非法：不完全指数 */
210f          /* 非法：没有小数或指数 */
.e55          /* 非法：缺少整数或小数 */
```

字符常量：字符常量是括在单引号里，例如，'x'，且可存储在一个简单的字符类型变量中。一个字符常量可以是一个普通字符（例如 'x'）、一个转义序列（例如 '\t'）或者一个通用字符（例如 '\u02C0'）。

在 C# 中有一些特定的字符，当它们的前面带有反斜杠时有特殊的意义，可用于表示换行符（\n）或制表符 tab（\t）


字符串常量：字符串常量是括在双引号 "" 里，或者是括在 @"" 里。字符串常量包含的字符与字符常量相似，可以是：普通字符、转义序列和通用字符

使用字符串常量时，可以把一个很长的行拆成多个行，可以使用空格分隔各个部分 

常量定义

```
public const int c1 = 5;
public const int c2 = c1 + 5;
```

> ### 运算符

1. 算数运算符

```
+加  -减  *乘  /除  %余  ++自加 --自减
```

2. 关系运算符

```
== 相等 !=不等 >大于 <小于 >=大于等于 <=小于等于
```

3. 逻辑运算符

```
&&与 ||或 !非
```

4. 赋值运算符

```
= += -= *= /= %= <<= >>= &= ^= |=
```

5. 其他运算符

```
sizeof() 返回数据类型大小
typeof() 返回类型
& 变量地址
* 变量指针
？ ： 三目运算符，条件表达式
is 判断对象是否某一类型
as 强制转换，即使转换失败也不抛异常
```

> ### 条件判断

if...else 
? :
switch

```
switch(expression){
    case constant-expression  :
       statement(s);
       break; 
    case constant-expression  :
       statement(s);
       break; 
  
    /* 您可以有任意数量的 case 语句 */
    default : /* 可选的 */
       statement(s);
       break; 
}
```

实例

```
public enum Rainbow
{
    Red, Orange, Yellow, Green, Blue, Indigo, Violet
}

public static void TestSwitch2(Rainbow colorBand)
{
    // switch 语句
    string word1;
    switch (colorBand)
    {
       case Rainbow.Red: word1 = "红"; break;
       case Rainbow.Orange: word1 = "橙"; break;
       case Rainbow.Yellow: word1 = "黄"; break;
       case Rainbow.Green: word1 = "绿"; break;
       case Rainbow.Blue: word1 = "蓝"; break;
       case Rainbow.Indigo: word1 = "靛"; break;
       case Rainbow.Violet: word1 = "紫"; break;
       default: throw new ArgumentException("Invalid enum value", nameof(colorBand));
    }
    // switch表达式[C#8新特性]
    // 1.变量在switch之前，2.case:语句替换为=>表达式，3.default事例替换为_弃元
    string word2 = colorBand switch
    {
        Rainbow.Red => "红",
        Rainbow.Orange => "橙",
        Rainbow.Yellow => "黄",
        Rainbow.Green => "绿",
        Rainbow.Blue => "蓝",
        Rainbow.Indigo => "靛",
        Rainbow.Violet => "紫",
        _ => throw new ArgumentException("Invalid enum value", nameof(colorBand)),
    };
    Debug.Log("word1:" + word1 + " " + "word2:" + word2);
}
```

> ### 循环控制语句

1. while 循环

```
int a = 10;
while (a < 20)
{
  Console.WriteLine("a 的值： {0}", a);
  a++;
}
Console.ReadLine();
```

2. for循环

```
for (int a = 10; a < 20; a = a + 1)
{
    Console.WriteLine("a 的值： {0}", a);
}
Console.ReadLine();
```

3. foreach循环

```
int[] fibarray = new int[] { 0, 1, 1, 2, 3, 5, 8, 13 };
foreach (int element in fibarray)
{
    System.Console.WriteLine(element);
}
System.Console.WriteLine();
```

[Unity 关于foreach产生GC的bug修复](https://blog.csdn.net/qq_37421018/article/details/102766985)   
[Upgraded C# Compiler on 5.3.5p8](https://forum.unity.com/threads/upgraded-c-compiler-on-5-3-5p8.417363/)

4. do...while

```
int a = 10;

do
{
   Console.WriteLine("a 的值： {0}", a);
    a = a + 1;
} while (a < 20);
Console.ReadLine();
```

5. break语句

* 可以终止switch语句中的一个case
* 终止当前循环

```
int a = 0;

while (a < 5)
{
    Console.WriteLine("{0}", a);
    a++;
    if (a > 3)
    {
        break;
    }
}
Console.ReadLine();
// 0 1 2 3
```

6. continue语句

结束当前循环，执行下一次循环

```
int a = 0;

do
{
    if (a == 3)
    {
        a = a + 1;
        continue;
    }
    Console.WriteLine("a 的值： {0}", a);
    a++;

} while (a < 5);
Console.ReadLine();
// 0 1 2 4
```

> ### 抽象 封装 访问修饰符

抽象和封装是面向对象程序设计的相关特性   
抽象允许相关信息可视化   
封装则使开发者实现所需级别的抽象，防止对实现细节的访问

访问修饰符定义了类成员的范围和可见性

public 所有对象都可以访问
internal 同一个程序集的对象可以访问
protected 只有该类对象及其子对象可以访问
private 对象本身在对象内部可以访问

protected internal 两者并集

> ### 方法与传值

##### 方法定义

```
<Access Specifier> <Return Type> <Method Name>(Parameter List)
{
   Method Body
}
```

* 静态方法 ``` static void MyStaticMethod() { } ```
* 构造方法 ``` public+无返回修饰+类名(Parameter List) { } ```
* 析构方法
* 虚方法
* 抽象方法

##### 参数传值

值参数：复制值作形参，形参和实参不同内存，形参改变不影响实参
引用参数：复制内存地址，形参改变影响实参
输出参数：形参改变影响实参 // 比较引用参数，输出参数可以只定义然后应用

```
// 值参数
public void swap(int x, int y)
{
 int temp;
 temp = x; 
 x = y;  
 y = temp;
}
// 引用参数       
public void swap(ref int x, ref int y)
{
 int temp;

 temp = x; /* 保存 x 的值 */
 x = y;    /* 把 y 赋值给 x */
 y = temp; /* 把 temp 赋值给 y */
}

public void getValues(out int x, out int y )
{
  Console.WriteLine("请输入第一个值： ");
  x = Convert.ToInt32(Console.ReadLine());
  Console.WriteLine("请输入第二个值： ");
  y = Convert.ToInt32(Console.ReadLine());
}
   
static void Main(string[] args)
{
 NumberManipulator n = new NumberManipulator();
 int a , b; // 可以不赋值
 n.getValues(out a, out b);
 Console.WriteLine("在方法调用之后，a的值{0} b的值： {1}", a, b);
 Console.ReadLine();
  }
```

方法简写 => 读作“goes to”

```
public string Name // Property 属性，对 Field进行封装
{
    get { return _name; }
    set { _name = value; }
}

可以简写为
public string Name // Property 属性，对 Field进行封装
{
    get => _name; // get { return _name; }
    set => _name = value; // set { _name = value; }
}
```

变长参数

```
// Sum(1, 2, 3, 4, 5);
static int Sum(params int[] paramArray)
{
	int sum = 0;
	for (int i = 0; i < paramsArray.Length; i++)
	{
		sum += paramArray[i];
	}
	return sum;
}
```

参数默认值

```
 static void Speak(string str = "没话说！")
{
    Debug.Log(str);
}
```

> ### 可空类型

```
int? num1 = null;
int num2 = num1 ?? 2
Console.WriteLine("{0}, num2") // 2
```

> ### 数组

* Array 

Array数组是最简单的数据结构，其特单为：

```
数组存储在连续的内存上，
数组的内容都是相同的类型，
数组可以直接通过下标访问。
```

创建一个新的数组时将在CLR托管堆中分配一块连续的内存空间，来盛放数量为size，类型为指定的类型。由于是在连续内存上存储的，所以他的索引速度非常快，访问指定下标时间与数组中元素数量无关。存储值类型不发生装箱操作

但是由于连续存储，所以在连个元素之间插入新的元素变得不方便，声明数组时必须指定长度。声明的长度过长，浪费内存；声明的长度过短，存在溢出风险。


1. 数组声明与赋值

```
// 索引赋值
double[] balances = new double[5]
balances[0] = 10.01
// 声明同时赋值
int[] ages = { 18, 19, 20}
// 创建并初始化
int[] ages = new int[3] { 18, 19, 20}
// 可以省略大小 int[] ages = new int[] { 18, 19, 20}
// 下标访问数组元素
int age = ages[0]
// for循环
for (int i = 0; i < ages. Length, i++) 
// foreach循环
foreach (int age in ages)
```

2. 数组遍历

```

int[] ages = new int[] { 18, 19, 20}
// for循环
for (int i = 0; i < ages. Length, i++) 
// foreach循环
foreach (int age in ages)

// 不定类型 用 foreach 遍历 var 数组
var people = new[] { new {Name = "Steven", Age = 27}, 
                     new {Name = "Lily", Age = 25}, 
                     new {Name = "Tom", Age = 8} }; 
foreach(var person in people) 
{ 
    Console.WriteLine(person);  
}
```

3. 数组扩容 

```
int[] arrs = new[] {1, 2, 3, 4, 5};
Array.Resize(ref arrs, arrs.Length + 1);
// 给扩容后原数组的最后一个位置赋值
arrs[arrs.Length - 1] = 6;
```

4. 数组拷贝

```
int[] pins = { 1, 2, 3, 4 }; 
int[] copy = new int[pins.Length]; 
pins.CopyTo(copy, 0);   // 将数组 pins 的内容复制到数组 copy 
// CopyTo 是浅拷贝，从指定位置开始复制，复制后两个数组都引用同一组对象 
   
int[] pins = { 1, 2, 3, 4 }; 
int[] copy = new int[pins.Length]; 
Array.Copy(pins, copy, copy.Length);    // Copy 方法是浅拷贝 
   
int[] pins = { 1, 2, 3, 4 }; 
int[] copy = (int[])pins.Clone();       // Clone 方法是浅拷贝 
    // Clone 放回的是一个 object，所以必须进行类型准换 
   
int[] pins = { 1, 2, 3, 4 }; 
int[] copy = new int[pins.Length]; 
for (int i = 0; i < copy.Length; i++) 
{ 
    copy[i] = pins[i];      // 深拷贝，也就是产生了两个数组 
} 
```

* ArrayList 动态数组

ArrayList可以使用索引在指定位置操作，且自动调整大小(不必声明时指定长度)，还可以存储不同类型元素，但必须引入System.Collections.

由于把所有元素当做Object处理，所以类型不安全，可能发生类型不匹配情况。ArrayList存储值发生装箱操作，索引取值发生拆箱操作。装箱是隐式的；拆箱必定是显式的

```
ArrayList list = new ArrayList();
list.Add(22);
list.Add("abc");
list[1] = "abcd";
list.RemoveAt(1);
```

常用属性

```
Capacity 获取可以包含的最大个数
Count 实际包含的元素个数
IsFixedSize ArrayList是否具有固定大小
IsReadOnly ArrayList是否只读
IsSynchronized ArrayList是否同步访问
SyncRoot 同步访问ArrayList
Item[Int32] 获取指定索引处元素
```

常用方法

```
public virtual int Add([CanBeNull] Object value) // 末尾添加对象
public virtual void AddRange(ICollection c) // 末尾添加ICollection的元素
public virtual void Clear() // 移除所有元素
public virtual bool Contains(Object item) // 是否包含某元素
public virtual ArrayList GetRange(int index, int count) // 获取范围内子集
public virtual int IndexOf(Object value) // 某个对象第一次出现的索引
public virtual void insert(int index, Object value) // 指定索引插入元素
public virtual void Remove(Object value) // 移除第一次出现的指定对象
public virtual void Reverse() // 反序
public virtual void Sort() // 排序
public virtual void TrimToSize() // 设置容量为元素实际个数
```

* List<T> 泛型数组

为了解决ArrayList类型不安全和装箱拆箱问题，所以出现泛型数组概念。
Unity中能很好序列化，推荐使用

```
List<string> list = new List<string>();
list.Add("abc");
list[0] = "def";
list.RemoveAt(0);
```

* LinkedList<T> 链表

链表在内存存储上可能不是连续的，通过上一个元素指向下一个元素来排列，所以不通过下标访问。

1. 向链表插入或者删除节点无需调整结构容量。
2. 链表适合在需要有序排列的情境下增加新的元素。数组插入可能移动很多元素，链表只需若干元素指向发生改变
3. 内存分布不连续，无法下标访问，而是必须从头节点开始，逐次遍历下一个节点直到找到目标，当需要快速访问时，数组反而有优势。

[微软官方文档](https://docs.microsoft.com/zh-cn/dotnet/api/system.collections.generic.linkedlist-1?redirectedfrom=MSDN&view=netframework-4.8)


* Queue<T> 队列

队列具有先进先出特点[FIFO]，又称先进先出线性表，通过使用Enqueue和Dequeue两个方法来实现对队列的存取。

1. 先进先出
2. 初始容量32，增长因子2
3. 当使用Enqueue时，会判断队列的长度是否足够

[微软官方文档](https://docs.microsoft.com/zh-cn/dotnet/api/system.collections.generic.queue-1?redirectedfrom=MSDN&view=netframework-4.8)

* Stack<T> 栈

1. 后进先出LIFO
2. 默认容量10
3. 使用pop和push操作

[微软官方文档](https://docs.microsoft.com/zh-cn/dotnet/api/system.collections.generic.stack-1?redirectedfrom=MSDN&view=netframework-4.8)


* Dictionary<K, T> 泛型字典

提到字典就不得不说Hashtable哈希表以及Hashing（哈希，也有叫散列的），因为字典的实现方式就是哈希表的实现方式，只不过字典是类型安全的，也就是说当创建字典时，必须声明key和item的类型，这是第一条字典与哈希表的区别。关于哈希表的内容推荐看下这篇博客哈希表。关于哈希，简单的说就是一种将任意长度的消息压缩到某一固定长度，比如某学校的学生学号范围从00000~99999，总共5位数字，若每个数字都对应一个索引的话，那么就是100000个索引，但是如果我们使用后3位作为索引，那么索引的范围就变成了000~999了，当然会冲突的情况，这种情况就是哈希冲突(Hash Collisions)了。

　　回到Dictionary<K,T>，我们在对字典的操作中各种时间上的优势都享受到了，那么它的劣势到底在哪呢？对嘞，就是空间。以空间换时间，通过更多的内存开销来满足我们对速度的追求。在创建字典时，我们可以传入一个容量值，但实际使用的容量并非该值。而是使用“不小于该值的最小质数来作为它使用的实际容量，最小是3。”（老赵），当有了实际容量之后，并非直接实现索引，而是通过创建额外的2个数组来实现间接的索引，即int[] buckets和Entry[] entries两个数组（即buckets中保存的其实是entries数组的下标），这里就是第二条字典与哈希表的区别，还记得哈希冲突吗？对，第二个区别就是处理哈希冲突的策略是不同的！字典会采用额外的数据结构来处理哈希冲突，这就是刚才提到的数组之一buckets桶了，buckets的长度就是字典的真实长度，因为buckets就是字典每个位置的映射，然后buckets中的每个元素都是一个链表，用来存储相同哈希的元素，然后再分配存储空间。

 因此，我们面临的情况就是，即便我们新建了一个空的字典，那么伴随而来的是2个长度为3的数组。所以当处理的数据不多时，还是慎重使用字典为好，很多情况下使用数组也是可以接受的

[哈希表](https://www.cnblogs.com/KingOfFreedom/archive/2012/12/11/2812505.html)

Dictionary提供快速的基于键值的元素查找。
结构是：Dictionary <[key] , [value] >,当你有很多元素的时候可以用它。
它包含在System.Collections.Generic名控件中。在使用前，你必须声明它的键类型和值类型

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace Demo1
{
    class Program
    {
        static void Main(string[] args)
        {
            //创建泛型哈希表,Key类型为int,Value类型为string
            Dictionary<int, string> myDictionary = new Dictionary<int, string>();
            //1.添加元素
            myDictionary.Add(1, "a");
            myDictionary.Add(2, "b");
            myDictionary.Add(3, "c");
            //2.删除元素
            myDictionary.Remove(3);
            //3.假如不存在元素则添加元素
            if (!myDictionary.ContainsKey(4))
            {
                myDictionary.Add(4, "d");
            }
            //4.显示容量和元素个数
            Console.WriteLine("元素个数：{0}",myDictionary.Count);
            //5.通过key查找元素
            if (myDictionary.ContainsKey(1))
            {
                Console.WriteLine("key:{0},value:{1}","1", myDictionary[1]);
                Console.WriteLine(myDictionary[1]);            
            }
            //6.通过KeyValuePair遍历元素
            foreach (KeyValuePair<int,string>kvp in myDictionary)
            {
                Console.WriteLine("key={0},value={1}", kvp.Key, kvp.Value);

            }
            //7.得到哈希表键的集合
            Dictionary<int, string>.KeyCollection keyCol = myDictionary.Keys;
                //遍历键的集合
                foreach (int n in keyCol)
                {
                    Console.WriteLine("key={0}", n);                
                }
            //8.得到哈希表值的集合
            Dictionary<int, string>.ValueCollection valCol = myDictionary.Values;
                //遍历值的集合
                foreach( string s in valCol)
                {
                Console.WriteLine("value：{0}",s);
                }
            //9.使用TryGetValue方法获取指定键对应的值
            string slove = string.Empty;
            if (myDictionary.TryGetValue(5, out slove))
            {
                Console.WriteLine("查找结果：{0}", slove);
            }
            else
            {
                Console.WriteLine("查找失败");
            }
            //10.清空哈希表
            //myDictionary.Clear();
            Console.ReadKey();
        }
    }
}
```

> ### 多态性

同一接口，使用不同实例而执行不同操作

##### 静态多态性

* 函数重载

方法名相同，参数不同(数量、顺序、类型等方面的不同)，重载与返回值类型无关

```
void Print(int a) 
{
	Console.WriteLine("输出: {0}", a );
}

void Print(int a, int b) 
{
	Console.WriteLine("输出: {0} {1}", a, b);
}

void Print(double a) 
{
	Console.WriteLine("输出: {0}", a );
}
``` 

* 运算符重载

```
sealed class Rectangle
{
    public double Length { get; private set; }
    public double Height { get; private set; }

    public void SetLength(double lenght)
    {
        Length = lenght;
    }
    public void SetHeight(double height)
    {
        Height = height;
    }

    #region 运算符重载 == 和 != 需要同时重载
    public static bool operator==(Rectangle lhs, Rectangle rhs)
    {
        return (lhs.length == rhs.length && lhs.height == rhs.height);
    }

    public static bool operator !=(Rectangle lhs, Rectangle rhs)
    {
        return !(lhs == rhs);
    }
    #endregion
}
```


* [数组扩容](https://www.cnblogs.com/darrenji/p/3978150.html)
* [Unity2020.2中支持的C#8有什么新特性](https://blog.csdn.net/zhenghongzhi6/article/details/111474513)
* [Unity各版本C#支持情况](https://blog.csdn.net/t163361/article/details/107266702/)
* [C#9](https://www.cnblogs.com/CoderAyu/p/11000386.html)
* [C#更新列表](https://github.com/dotnet/csharplang/blob/master/Language-Version-History.md)
* [C#documentation](https://docs.microsoft.com/en-us/dotnet/csharp/)
* [C#参考](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/)
* [Unity脚本运行时更新 C#7.2新特性](https://blog.csdn.net/zhenghongzhi6/article/details/83055293)
* [Unity脚本运行时更新 C#7.1新特性](https://blog.csdn.net/zhenghongzhi6/article/details/83010669)
* [Unity脚本运行时更新 C#7新特性](https://blog.csdn.net/zhenghongzhi6/article/details/82953184)
* [Unity脚本运行时更新 C#6新特性](https://blog.csdn.net/zhenghongzhi6/article/details/82882462)
* [Unity脚本运行时更新 C#5新特性](https://blog.csdn.net/zhenghongzhi6/article/details/82797533)
* [Unity脚本运行时更新 C#4新特性](https://blog.csdn.net/zhenghongzhi6/article/details/82776896)
* [Unity C#基础](https://blog.csdn.net/qq_42481369?t=1)
* [Unity中常用的数据结构](https://blog.csdn.net/anbd0604/article/details/101430701)