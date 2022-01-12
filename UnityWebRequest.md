> ### 为什么弃用WWW，转而用UnityWebRequest?

因为WWW不具备timeout属性，当网络不好时无法准确做出超时判断。网络极差时，下载可能一直等待 yield return www。另外性能也极差，GC消耗高。

Unity5.4 推出UnityWebRequest，Unity2018废弃WWW

> ### UnityWebRequest介绍

UnityWebRequest支持上传、下载、断点续传等功能。   
UploadHandler处理将数据上传到服务器   
DownloadHandler 从服务器下载数据   
UnityWebRequest 负责与Http通信并管理上面两个对象   

SendWebRequest() 开始与服务器通信，调用此方法后，有必要时UnityWeRequest将执行DNS解析，将Http请求发送到目标URL的远程服务器并处理响应   
Get(url) 创建一个Http为传入URL的UnityWebRequest的对象   
Post(ulr) 向服务器发送Post表单数据  
Put(url) 将数据上传到服务器   
Abort() 直接结束联网
Head() 创建一个为传输Http请求头的UnityWebRequest对象   
GetResponseHeader() 返回一个字典，内容为在最新的Http响应中收到的所有响应头 

> ### 构造方法

```
public UnityWebRequest();
public UnityWebRequest(Uri uri);
public UnityWebRequest(Uri uri, string method)
public UnityWebRequest(Uri uri, string method, Networking.DownloadHandler downloadHandler, Networking.UploadHandler uploadHandler);
```

method: 相当于请求方法名称，GET/POST/PUT/HEAD，默认GET，一旦调用SendWebRequest()就无法修改   

> ### GET请求

```
using System;
using System.Collections;
using UnityEngine;
using UnityEngine.Networking;

namespace UnityTestProject
{
    public class TestUnityWebRequest : MonoBehaviour
    {
        private void Start()
        {
            StartCoroutine(SendGetRequest());
        }
        // ReSharper disable Unity.PerformanceAnalysis
        /// <summary>
        /// 开启一个协程发送GET网络请求
        /// </summary>
        /// <returns></returns>
        IEnumerator SendGetRequest()
        {
            Uri uri = new Uri("https://www.baidu.com"); // using System
            UnityWebRequest request = new UnityWebRequest(uri);
            request.timeout = 5;
            yield return request.SendWebRequest();
            // request.isHttpError || request.isNetworkError
            if (request.result == UnityWebRequest.Result.ProtocolError)
            {
                Debug.LogError("请求失败，HttpProtocolError: " + request.error);
            }
            else if (request.result == UnityWebRequest.Result.ConnectionError)
            {
                Debug.LogError("请求失败，HttpConnectionError: " + request.error);
            }
            else
            {
                Debug.Log("请求成功");
                /*
                 如果访问的链接有返回文本结果，如json文本，则通过text获取
                 string reusltStr = request.downloadHandler.text;
                 如果访问的是资源链接，比如图片，则通过data拿到图片二进制流
                 byte[] data = request.downloadHandler.data;
                 */
            }

        }
    }
}
```

> ### POST请求

```
IEnumerator SendPostRequest()
{
    WWWForm form = new WWWForm();
    form.AddField("name", "LiBai");
    form.AddField(fieldName:"age", value: "18");
    form.AddField("Score", 99);
    UnityWebRequest request = UnityWebRequest.Post(uri:"https://example.com", form);
    yield return request.SendWebRequest();
    if (request.isHttpError || request.isNetworkError)
    {
        Debug.Log(request.error);
    }
    else
    {
        Debug.Log("上传成功");
    }
}
```

> ### PUT请求

```
IEnumerator SendPutRequest()
{
    byte[] data = System.Text.Encoding.UTF8.GetBytes("TestData");
    using (UnityWebRequest request = UnityWebRequest.Put("heeps://www.example.com", data))
    {
        yield return request.SendWebRequest();
        if (request.result == UnityWebRequest.Result.ProtocolError ||
            request.result == UnityWebRequest.Result.ConnectionError)
        {
            Debug.Log(request.error);
        }
        else
        {
            Debug.Log("上传成功");
        }
    }
}
```

> ### HEAD请求以及GetResponseHeader获取文件长度



> ### Abort方法

Abort方法会尽快结束联网，可以随时调用此方法。   
如果UnityWebRequest未完成，那么UnityWebRequest将尽快停止上传或下载数据。中止的UnityWebRequest被认为遇到了系统错误。
isNetworkError 和 isHttpError将返回true[request.result == UnityWebRequest.Result.ProtocolError ||
            request.result == UnityWebRequest.Result.ConnectionError]，error = ”User Aborted“

<!-- https://blog.csdn.net/linxinfa/article/details/94436027 -->

* [Unity 简单封装UnityWebRequest](https://www.jianshu.com/p/3da9191f82a0)
* [Unity实现断点续传下载功能](https://www.blinkedu.cn/index.php/2021/08/19/unity实现断点续传下载功能/)