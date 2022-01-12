```
namespace DDYTest
{
    public class TestReachability
    {
        // Start is called before the first frame update
        public static void StartCheckNetReachability()
        {
            if (Application.internetReachability == NetworkReachability.NotReachable)
            {
                Debug.Log("Unity自带判断，没有联网");
            }
            else if (Application.internetReachability == NetworkReachability.ReachableViaCarrierDataNetwork)
            {
                Debug.Log("Unity自带，连接的移动网络");
            }
            else if (Application.internetReachability == NetworkReachability.ReachableViaLocalAreaNetwork)
            {
                Debug.Log("Unity自带，连接的wifi");
            }
            else
            {
                Debug.Log("Unity自带判断，已经联网");
            }

            //PingNetAddress();
        }

        public static bool PingNetAddress(string address = "http://www.baidu.com")
        {
            bool flag = false;
            Ping ping = new Ping();
            try
            {
                PingReply pingReply = ping.Send(address, 3000);
                if (pingReply.Status == IPStatus.Success)
                {
                    flag = true;
                }
                else
                {
                    flag = false;
                }
            }
            catch (Exception e)
            {
                flag = false;
                Debug.Log($"{e.Message}");
            }

            Debug.Log($"Ping结果{flag}");
            return flag;
        }
    }
}
```

* [Unity3D 原生网络状态API跳坑笔记](https://www.jianshu.com/p/17d8cfc18a85)
* [unity 判断网络状态](https://blog.csdn.net/bamboo_yayaya/article/details/103162081)
* [Unity检测网络连接状态](https://blog.csdn.net/qq_38721111/article/details/103385580)