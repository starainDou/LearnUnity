> ### 关闭灯光自动烘焙线程

window -> Lighting -> Setting -> Auto Generate 取消勾选

```
using UnityEditor;
using UnityEditor.SceneManagement;
using UnityEngine.SceneManagement;
 
[InitializeOnLoad]
public static class EditorDisableAutoGenerateLighting
{
    static EditorDisableAutoGenerateLighting()
    {
        EditorSceneManager.sceneOpened += OnSceneOpend;
        EditorSceneManager.newSceneCreated += OnSceneNewCreate;
    }
 
    private static void DisableAutoGenerateLighting()
    {
        Lightmapping.giWorkflowMode = Lightmapping.GIWorkflowMode.OnDemand;
    }
 
    public static void OnSceneOpend(Scene scene, OpenSceneMode mode)
    {
        DisableAutoGenerateLighting();
    }
 
    public static void OnSceneNewCreate(Scene scene, NewSceneSetup setup, NewSceneMode mode)
    {
        DisableAutoGenerateLighting();
    }
}
```
<!-- https://blog.csdn.net/lsjsoft/article/details/103931894 -->


> ### 关闭垂直同步

Edit -> Project Settings -> Quality -> VSync Count -> Don't Sync

```
Don’t Sync（关闭VSync）
Every VBlank（每个VBlank计算一帧）
Every Second VBlank（每两个VBlank计算一帧）
```

tip: 显示帧率

```
using System.Collections;
using UnityEngine;
 
public class Test : MonoBehaviour
{
    float updateInterval = 0.5f;
    private float accum = 0.0f;
    private float frames = 0;
    private float timeleft;
 
    void Start()
    {
        timeleft = updateInterval;
    }
 
 
    void Update()
    {
        timeleft -= Time.deltaTime;
        accum += Time.timeScale / Time.deltaTime;
        ++frames;
 
        if (timeleft <= 0.0)
        {
            Debug.Log( "" + (accum / frames).ToString("f2"));
            timeleft = updateInterval;
            accum = 0.0f;
            frames = 0;
        }
    }
}
```

* [Unity优化大全（四）之CPU- VSync Count垂直同步](https://gameinstitute.qq.com/community/detail/120791)

> ### Log日志

1. Release环境只打印 error或者不打印（Debug.Log会产生GC，影响性能）

```
if (Debug.isDebugBuild) {
    Debug.logger.logEnabled = true;
    //Debug.logger.filterLogType = LogType.Log; //显示所有
} else {
    Debug.logger.logEnabled = false;
    //Debug.logger.filterLogType = LogType.Error;/／只显示Error + Exception
}
```
[Unity中log优化](https://blog.csdn.net/qq_33337811/article/details/77916336)

2. Conditional 条件特性方式

采用条件编译标签Conditional封装一层自己的Log输出，来直接避免掉Log输出的编译，还可以省去Log函数参数传递和调用的开销

```
//伪代码
public static class DebugEx {
     //需要在Unity PlayerSettings -> Scripting Define Symbol 添加VERBOSE
     //Release发布时去掉VERBOSE，所有的DebugEx.Log函数调用将不会参与编译。
     //这是简单的实现方式 , 后期功能的扩展以及Debug和Release自动切换机制，可以开发者进行封装。
     [Conditional("VERBOSE")] 
     public static void Log(object message, UnityEngine.Object obj = null) {
         UnityEngine.Debug.Log(message, obj);
     }

     public static void LogWarning(object message, UnityEngine.Object obj = null) {
         UnityEngine.Debug.LogWarning(message, obj);
     }

     public static void LogError(object message, UnityEngine.Object obj = null) {
         UnityEngine.Debug.LogError(message, obj);
     }
 }
```

3. 使用免费插件 Log Viewer

```
https://assetstore.unity.com/#!/content/12047
```

> ### 小技巧

```
1. 尽量不要在 Update 中使用 GetComponent/ GameObject.Find等，
而是在 Avake/Start 中使用，然后用属性缓存

2. 运算少用“除”，多用“加、减、乘”， 根据Unity官方数据 ：除需要30–40 cycles来完成，加/减/乘 只需要一两个，平方根、Sin、Cos 需要60–100个

3. 单纯比较向量距离，少用Normalize：Normalize = vec / sqrt( vec.x^2 + vec. y^2 + vec. z^2)，所以尽量改用sqrMagnitude

4. 多使用Trigger或Event delegeate来触发或通知状态的改变，而非每个每个Update作检查

5. 尽量避免每个Frame作Raycasting：对於Mesh物件作Raycasting有一定的Cost， 所以尽量避免每个Frame作Raycasting，可用Culling (Layer) mask先滤掉不必要的物件
```

* [Unity3D性能优化——CPU篇](https://zhuanlan.zhihu.com/p/39998137)
* [Unity性能优化之内存-贴图格式优化](https://segmentfault.com/a/1190000015415791)
* [unity profile使用，内存优化，包大小优化](https://blog.51cto.com/chenshuhb/1836631)
* [马三小伙儿 性能优化相关专题](https://github.com/XINCGer/Unity3DTraining/tree/master/PerformanceOptimization)
* [unity在ios平台下内存的优化？](https://www.zhihu.com/question/26779558/answer/34015434)
* [Unity3D性能优化——CPU篇](https://zhuanlan.zhihu.com/p/39998137)
* [好插件让你事半功倍 Mesh 合并](https://www.jianshu.com/p/f01cf62f12a9)
* [丢掉Mask遮罩，更好的圆形Image组件[Unity]](https://www.cnblogs.com/leoin2012/p/6425089.html)
* [[Unity] UGUI远离Mask，实现圆形的Image](https://www.jianshu.com/p/8eaa545418b1)
* [UGUI Panel的显示和隐藏优化](https://blog.csdn.net/qq_38721111/article/details/119802589)
* [Unity优化技巧进阶](https://blog.csdn.net/AndrewFan/article/details/59057811)
* [为C#定制联合(Union)提高序列化速度](https://blog.csdn.net/AndrewFan/article/details/62227628)
* [UGUI优化之路- Image的Sliced优化](https://blog.csdn.net/qq_19929447/article/details/114786051)