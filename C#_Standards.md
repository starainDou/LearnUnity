> ### 规范目的

1. 一个软件的声明周期，80%的时间花费在维护中；
2. 很少有软件在整个生命周期中完全由最初的开发人员维护；
3. 编码规范可以改善软件的可读性，让维护人员尽快理解代码；
4. 好的编码约定提升代码质量，减少出问题概率。

> ### 代码注释

##### 代码注释约定

1. 所有类，方法，属性等都应该添加简明注释，以说明其功能
2. 当功能代码较为难理解时也应该添加注释

##### 模块头部注释规范

```
// ********************
// 文件(File Name): Example.cs
// 作者(Author): RainDou
// 日期(Create Date): 2021.01.01
// ********************
``` 

##### 方法注释规范

C#提供一种机制，使开发人员可以使用包含XML文本的特殊注释语法为代码编写文档

```
<c> 提供了一种将说明中的文本标记为代码的方法
<code> 提供了一种将多行指示为代码的方法
<example> 可以指定使用方法或其他库成员的示例，一般涉及到<code> 标记的使用
<exception> 对可从当前编译环境中获取的异常的引用
<include> 得以引用描述原代码中类型和成员的另一文件中的注释
<list> 用于定义表或定义列表中的标题行
<para> 用于诸如<summary>、<remark>、<returns>等标记内，使得以将结构添加到文本中
<param>用于方法声明的注释中，描述一个参数
<permission>得以将成员的访问记入文档
<remarks> 用于添加有关某个类型的信息，从而补充由<summary>所指定的信息
<returns> 用于方法声明的注释中，描述返回值
<see> 得以从文本内指定链接
<sesalso> 对可以通过当前编译环境进行调用的成员或字段的引用
<summary> 描述类型或类型成员
<value> 得以描述属性
```

示例

```
/// <summary>
/// 使用MD5计算哈希散列值
/// </summary>
/// <param name="strSource">需要加密的字符串</param>
/// <returns>加密后的字符串返回值</return>
public static string EncryMD5(string strSource) 
{
	System.Security.Cryptography.MD5CrytoServiceProvider md5 = new System.Security.Cryptography.MD5CryptoServiceProvider();
	// MD5 md5 = MD5.Create(); // 系统封装
	byte[] bs = Encoding.UTF8.GetBytes(strObj);
	byte[] hs = md5.ComputeHash(bs);
	StringBuilder sb = new StringBuilder();
	foreach (var VARIABLE in hs)
	{
	    // 以十六进制格式化
	    sb.Append(VARIABLE.ToString("x2"));
	}
	return sb.ToString();
}
```

##### 行注释规范

1.如果某个代码段有很多逻辑结构，或难以理解，此时应加行注释，以说明处理思路或注意事项
2.行注释用 //+空格+注释 与代码左对齐

```
// 如果连接已经打开，则关闭连接并释放资源
if (this.IDcConnection.State == ConnectionState.Open) 
{
	this.IDbConnection.Close();
	this.IDbConnection.Dispose();
}
```

##### 变量注释规范

1. Class 级变量采用 /// 产生XML标签格式注释

```
/// <summary>
/// 数据服务器名称
/// </summary>
private static string dbServerName = "";
```

2. 方法级变量 可以放在变量声明后

```
protected bool sendNotification(String strSubJectResID) 
{
	bool isSuccess = false; // 是否发送成功
}
```

> ### 命名规范

##### 基本约定

1. 使用可以准确说明变量、字段、类的完整英文描述，如 firstName。对于过于简单的，如循环里递增变量，可以用i
2. 尽可能使用项目涉及领域的术语
3. C#中尽可能不采用下划线分割形式
4. 避免使用过于相似或仅大小写区别的名字
5. 避免过于冗余，过于长的名字
6. 尽量避免缩写，除非一些常用通用缩写
7. 不同类型标识符大小写规则

```
命名空间 Pascal 如 namespace Com.Techstar.ProductionCenter
类型 Pascal 如 public class Person
接口 Pascal 如 public interface ITableModel
方法 Pascal 如 public void UpdateData()
属性 Pascal 如 public int FirstName
事件 Pascal 如 public event EventHandler Changed;
枚举 Pascal 如 enum MessageType { LeaveGame, EnterGame }
非私有字段 Pascal 如 public string FieldName;
私有字段 Camel 如 private stirng _fieldName;
参数 Camel 如 public void UpdateDate(string currentDate) 
局部变量 Camel 如 string tempName;
泛型 Pascal 如 public class MyClass<T> // 或T开头描述文本 Tsession
```

<!-- https://www.cnblogs.com/zhaoshujie/p/9594688.html -->
<!-- https://blog.csdn.net/kasama1953/article/details/52196078 -->