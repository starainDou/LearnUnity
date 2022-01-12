> # Goolge.Protobuf

### 编译器 protoc

* 查看可用版本 ``` brew search protobuf ```
* 默认安装最新 ``` brew install protobuf ```
* 安装指定版本 ``` brew install protobuf@3.7 ```
* 连接指定版本 ``` brew link --force --overwrite protobuf@3.7 ```
* 查看安装信息 ``` brew info protobuf ```
* 查看安装版本 ``` protoc --version ```

### 运行时 Google.Protobuf

* NuGet 获取 Google.Protobuf
* 找到 Google.Protobuf.dll 并添加到 Assets/Plugins/Protobuf
* 找到剩下的几个依赖动态库也添加到上面目录(如 System.Buffers.dll System.Memory.dll System.Numerics.Vectors.dll System.Runtime.CompilerServices.Unsafe.dll)

### 使用

* 创建 Person.proto ``` touch Person.proto ```
* 打开 Person.proto ``` open -e Person.proto ```
* 内容

```
syntax="proto3";
package  tutorial;
 
import "google/protobuf/timestamp.proto";
 
message Person
{
	string name=1;
	int32 id=2;
	string email=3;
	enum PhoneType
	{
		HOME=0;
		WORK=1;
		MOBILE=2;
	}
	message PhoneNumber
	{
		string number=1;
		PhoneType type=2;
	}
	repeated PhoneNumber phones=4;
	google.protobuf.Timestamp last_update=5;
}
```

* 转换成Person.cs文件 ``` protoc --csharp_out=./ Person.proto  ```
注：

	```
	终端输入: $ protoc --proto_path=A --objc_out=B person.proto
	其中--proto_path=后跟A是需要处理的proto文件所在的文件夹，--objc_out=指明生成的是Objective-C代码以及目标文件存放路径，B是目标文件存放路径,person.proto是需要处理的文件
	```

* 将Person.cs加入工程中就可以使用了

```
using System.IO;
using UnityEngine;
using Google.Protobuf; // pb
using Tutorial; // 自定义Person模型

namespace  DDYTest
{
    public class TestProtobuf
    {
        public static void TestPerson()
        {
            Person rain = new Person
            {
                Id = 6666,
                Name = "RainDou",
                Email = "634778311@qq.com",
                Phones = { new Person.Types.PhoneNumber { Number = "18966666666", Type = Person.Types.PhoneType.Home } }
            };
            Debug.Log($"1 Name:{rain.Name} Id:{rain.Id} Email:{rain.Email} {rain.Phones[0].Type} phone:{rain.Phones[0].Number}");
            // serializztion
            using (var output = File.Create("john.dat"))
            {
                rain.WriteTo(output);
            }
            // parsing 
            using (var input = File.OpenRead("john.dat"))
            {
                Person john = Person.Parser.ParseFrom(input);
                Debug.Log($"2 Name:{john.Name} Id:{john.Id} Email:{john.Email} {john.Phones[0].Type} phone:{john.Phones[0].Number}");
            }

            byte[] result = rain.ToByteArray();
            Debug.Log($"序列化后数组长度{result.Length}");
            Person newPerson = Person.Parser.ParseFrom(result);
            Debug.Log($"3 Name:{newPerson.Name} Id:{newPerson.Id} Email:{newPerson.Email} {newPerson.Phones[0].Type} phone:{newPerson.Phones[0].Number}");
        }
    }
}
```

> # Protobuf-net

#### 运行时

[源码地址](https://github.com/protobuf-net/protobuf-net)

* NuGet 获取 Google.Protobuf
* 找到 protobuf-net.dll 并添加到 Assets/Plugins/ProtobufNet
* 找到依赖动态库也添加到上面目录 System.Collections.Immutable.dll

#### 使用

```
using System.IO;
using UnityEngine;
using ProtoBuf; // ProtoBuf-net

namespace  DDYTest
{
    [ProtoBuf.ProtoContract]
    public class NetPerson
    {
        //添加特性，表示该字段可以被序列化，1可以理解为下标
        [ProtoMember(1)]    
        public int ID;
        [ProtoMember(2)]
        public string Name;
        [ProtoMember(3)]
        public float Score;
    }
    public class TestProtobuf
    {
        public static void TestPerson()
        {
            #region Protobuf-net
            NetPerson netPerson = new NetPerson
            {
                ID = 100086,
                Name = "NetRain",
                Score = 99.5f
            };
            using (var file = File.Create("rain_protobuf_net.bin"))
            {
                Serializer.Serialize(file, netPerson);
            }

            using (var file = File.OpenRead("rain_protobuf_net.bin"))
            {
                NetPerson personNet = Serializer.Deserialize<NetPerson>(file);
                Debug.Log($"5 Name:{personNet.Name} Id:{personNet.ID} score:{personNet.Score} {personNet.Score}");
            }
            #endregion
        }
    }
}
```



* [Protocol Buffer 序列化原理大揭秘 - 为什么Protocol Buffer性能这么好？](https://blog.csdn.net/carson_ho/article/details/70568606)
* [在Unity中使用Protobuf](https://blog.csdn.net/qq_36458268/article/details/81067280)
* [Unity常见的解析数据方式XML,JSON,ProtocolBuf篇（一）Protobuf](https://blog.csdn.net/penchaoo/article/details/53609559)
* [如果想要自己编译指定版本的protobuf，并进行安装，可以参考](https://blog.csdn.net/qq_21383435/article/details/81035852)
* [Mac端安装protobuf及其简单使用](https://blog.csdn.net/love666666shen/article/details/89228450)
* https://stackoverflow.com/questions/21775151/installing-google-protocol-buffers-on-mac
* [Mac下ProtocolBuffer运行环境配置及编译测试](https://blog.csdn.net/qianlima210210/article/details/104889678)
* [系列 Protobuf3 教程 ](https://www.kaifaxueyuan.com/basic/protobuf3/protocol-buffer-csharp-compiling-your-protocol-buffers.html)
* [C# UnityWebRequest 使用Protocol Buffer样例](https://blog.csdn.net/qq_15555767/article/details/77994059)
* [Unity使用Protocolbuffer传输的字节序大小端问题](https://www.jianshu.com/p/20fd6db91e7a)
* [MAC使用ProtocolBuffer](https://www.jianshu.com/p/e217b1bb3c42)
* [Mac配置ProtocolBuffer环境及使用](https://www.jianshu.com/p/1bf89a8f7dd4)
* [在Unity中使用protobuf](https://zhuanlan.zhihu.com/p/75510885)
* [C#下使用protobuf](https://blog.csdn.net/yuxikuo_1/article/details/51494286)
* 插件protobuf-unity：https://github.com/5argon/protobuf-unity
* 插件protobuf_for_unity命令行 https://github.com/GongFaxin/protobuf_for_unity 或 https://github.com/bitcraftCoLtd/protobuf3-for-unity
* [Unity中使用ProtocolBuffer](https://www.cnblogs.com/Firepad-magic/p/14766483.html)
* [Google.Protobuf 入门详解](https://blog.csdn.net/q__y__L/article/details/86740156)
* [【Unity】基于ProtoBuffer与Socket实现网络传输](https://blog.csdn.net/treepulse/article/details/53465137)
* [unity中使用protobuf-net](https://blog.csdn.net/kenkao/article/details/80761627)
* [Unity中使用Protobuf-net源码](https://www.itdaan.com/tw/97680fdb53561ce7ea4e50f214580638)


/*
required：一个格式良好的消息一定要（至少）含有1个这种字段，表示该值是必须要设置的；
optional：每个消息中可以包含0个或多个optional类型的字段。
repeated：在一个格式良好的消息中，这种字段可以重复任意多次（包括0次）。重复的值的顺序会被保留，表示该值可以重复，相当于java中的List，golang中的切片
*/