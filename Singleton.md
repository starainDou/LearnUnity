```
public sealed class DDYWebSocket
{
    private static readonly Lazy<DDYWebSocket> _lazy = new Lazy<DDYWebSocket>(() => new DDYWebSocket());

    private DDYWebSocket()
    {
        
    }

    public static DDYWebSocket Instance
    {
        get
        {
            return _lazy.Value;
        }
    }
}
```

> ### 利用泛型，反射，抽象类抽取成模板

```
/*
 * Description: 单例模式模板
 * Mountpoint: 利用反射、泛型、抽象类实现
 * Author: DouDianYu
 */

using System;
using System.Reflection;

namespace DDYKit
{
    public abstract class DDYSingleton<T> where T : DDYSingleton<T>
    {
        private static readonly Lazy<T> _lazyInstance = new Lazy<T>(() =>
        {
            // 获取所有非public的构造方法
            var infos = typeof(T).GetConstructors(BindingFlags.Instance | BindingFlags.NonPublic);
            // 获取无参构造方法
            var ctor = Array.Find(infos, c => c.GetParameters().Length == 0);
            return ctor.Invoke(null) as T;
        });

        protected DDYSingleton()
        {
            
        }

        public static T Instance()
        {
            return _lazyInstance.Value;
        }
    }
}

/*
using DDYKit;

public class TestSingleton : DDYSingleton<TestSingleton>
{
    public string name;

    private TestSingleton()
    {
    
    }
}

TestSingleton.Instance().name = "我是单例";
Debug.Log(TestSingleton.Instance().name);
*/
```

https://www.cnblogs.com/ashbur/p/12054744.html
https://www.jianshu.com/p/97e4758ff4b4
https://www.zhihu.com/people/liangxiegame/posts?page=12
[Unity中单例模式的使用](https://blog.csdn.net/ycl295644/article/details/49487361)