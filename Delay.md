> ### Unity中延迟执行

* 协程方式

```
void Start()
{
    Debug.Log("测试延迟执行");
    StartCoroutine(DelayLogByCoroutine());
}
/// <summary>协程方式延迟</summary>
private IEnumerator DelayLogByCoroutine()
{
    yield return new WaitForSeconds(2.0f);
    Debug.Log("协程方式打印");
}
```

* Invoke方式

```
void Start()
{
    Debug.Log("测试延迟执行");
    Invoke("DelayLogByInvoke", 1.0f);
    // CanceInvoke("methodName"); // 取消所有名为methodName的调用
    // InvokeRepeating("LaunchProjectile", 2, 0.3F);  // 2秒后，每0.3f调用一次
}
/// <summary>Invoke调用延迟</summary>
private void DelayLogByInvoke()
{
    Debug.Log("Invoke方式打印");
}
```

> ### C# 中延迟

* Thread-Task

```
public static void StartTest()
{
    Thread thread = new Thread(new ParameterizedThreadStart(DelayLoad));
    thread.Start(3000);
}

// 不适用Unity中GameObject相关操作使用
private static async void DelayLoad(object obj)
{
    if (obj is int) // obj.GetType() == typeof(AssetReference)
    {
        try
        {
            var delayTime = (int) obj;
            await Task.Delay(delayTime);
            Debug.Log($"C#延迟{delayTime}milliseconds");
        }
        catch (OperationCanceledException)
        {
            Debug.Log("DelayLoad Cancel");
        }
    }
    else
    {
        Debug.Log("DelayLoad 类型不匹配");
    }
}
```