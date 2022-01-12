### * Quit

```
public void Quit ()
{
	#if UNITY_EDITOR // 编辑器模式下退出
	UnityEditor.EditorApplication.isPlaying = false;
	#else
	Application.Quit();
	#endif
}
```

### * 代码创建prefab

```
// 利用代码创建prefab
using UnityEngine;
using System.Collections;
using UnityEditor;
using System.IO;


public class CameraMove : MonoBehaviour {
    //生成Prefab， 参数1 需要生成的对象，参数2，名字
    private void CreatePrefabObj(GameObject obj, string prefabName) 
{
string[] str = prefabName.Split ('(');
string path = "Assets/Resources/_Art/Role/Individual/" + str[0] + ".prefab";
        //参数1 创建路径，参数2 需要创建的对象， 如果路径下已经存在该名字的prefab，则覆盖
PrefabUtility.CreatePrefab (path, obj);
    }
}
```

### * 默认新建2D、3D 切换

Edit -> Project Setting -> Editor -> Default Behaviour Mode

### * 切换正交相机和透视相机

Heriarchy 选中相机，然后Inspector -> Projection 

perspective: 透视相机，近大远小
orthographic 正交相机，远近一样

### * 扇形检测

```
public Transform Target;
public float SkillDistance = 5;//扇形距离
public float SkillJiaodu = 60;//扇形的角度

void LateUpdate()
{
    float distance = Vector3.Distance(transform.position, Target.position);//距离
    Vector3 norVec = transform.rotation * Vector3.forward * 5;//此处*5只是为了画线更清楚,可以不要
    Vector3 temVec = Target.position - transform.position;
    Debug.DrawLine(transform.position, norVec, Color.red);//画出技能释放者面对的方向向量
    Debug.DrawLine(transform.position, Target.position, Color.green);//画出技能释放者与目标点的连线
    float jiajiao = Mathf.Acos(Vector3.Dot(norVec.normalized, temVec.normalized)) * Mathf.Rad2Deg;//计算两个向量间的夹角
    if (distance < SkillDistance)
    {
        if (jiajiao <= SkillJiaodu * 0.5f)
        {
            Debug.Log("在扇形范围内");
        }
    }
}
```

### * 字符串数组转字符串，字符串分割成字符串数组

```
 string str = "1,2,3,4,5,6,7";
string[] strArray = str.Split(','); //字符串转数组
str = string.Empty;
str = string.Join(",", strArray);//数组转成字符串
```

### * Dictionary 减少多余访问

Dictionary使用时，我们常先用ContainsKey后再访问，相当于两次访问，用TryGet替代会更好。

### * List替代性优化

我们写程序时常用到List，但如果只是顺序访问[比如只是存值后遍历]，可用Queue或Stack代替List。

### * Panel显示与隐藏优化

1. SetActive(true/false) 需要其他脚本控制，且重新显示性能消耗很大
2. Scale 0,0,0 与 1,1,1切换，draw call加倍，大量GC
3. 移除Canvas父物体 draw Call加倍，大量GC，增加引用耦合
4. 放在Camera某个Culling层上，draw call加倍，大量GC，只对screen space-camera有效
5. Canvas.enable = false 延迟严重，且不方便
6. 给panel加一个CanvasGroup

```
// 若要显示
GetComponent<CanvasGroup>().alpha = 1;
GetComponent<CanvasGroup>().interactable = true;
GetComponent<CanvasGroup>().blocksRaycasts = true;
// 若要隐藏
GetComponent<CanvasGroup>().alpha = 0;
GetComponent<CanvasGroup>().interactable = false;
GetComponent<CanvasGroup>().blocksRaycasts = false;
```
[UGUI Panel的显示和隐藏优化](https://blog.csdn.net/qq_38721111/article/details/119802589)

### * 四元数和欧拉角转换

四元数转化欧拉角 Vector3 v3=transform.rotation.eulerAngles;   
欧拉角转换四元数 Quaternion rotation = Quaternion.Euler(v3);

### * 字符串拼接

1. 固定数量的字符串连接 + 的效率是最高的；
2. 字符串的数量不固定，且子串长度小于8，用StringBuiler效率高些。
3. 字符串的数量不固定，且子串长度大于8，用List<string>效率高些。
[C#字符串连接的效率问题](https://www.jianshu.com/p/7e5fbe75780d)

### * 避免生成meta文件

文件后加波浪线 ~ 可避免工程识别，就不会生成 meta文件 Test.png~

### * 震动

Handheld.Vibrate();

### * 剪切板

```
GUIUtility.systemCopyBuffer = "新剪贴板文本";
Debug.Log($"剪贴板文本 = {GUIUtility.systemCopyBuffer}");
```

### * 代码更改Image渲染顺序

```
// 最后渲染，执行后放到最上面
image1.transform.SetAsLastSibling();
// 最先渲染，执行后放到最下面
image1.transform.SetAsFirstSibling();
// 自定义层级显示，数值越大越后渲染。负数时等于SetAsFirstSibling
image1.transform.SetSiblingIndex(10);
```

### * 获取类型

```
// gameobject.GetType()
GetType() 
// typeOf(string)
typeOf()
```

### * 使用UniRx提升Update可读性

```
// 监听鼠标左键
Observable.EveryUpdate().Subscribe(_ =>
{
  if (Input.GetMouseButtonDown(0))
  {
      Debug.Log("left mouse button clicked");
  }
});

// 监听鼠标右键
Observable.EveryUpdate().Subscribe(_ =>
{
  if (Input.GetMouseButtonDown(1))
  {
      Debug.Log("Right mouse button clicked");
  }
});
```

### * GameObject或MonoBehaviours销毁时销毁UniRx任务

```
Observable.Timer(TimeSpan.FromSeconds(1f)).Subscribe().AddTo(this); // gameObject
```

### * C#中out与ref

* 共同点 都是传递引用（内存地址），使用后都将改变原来参数的数值。
* 不同点 ref是有进有出，out是只出不进: ref可以把参数的值传入函数
* out 可以不先初始化

[C#中out和ref之间的区别](https://www.jianshu.com/p/3d23e20535cb)

### * 判断脚本是否继承了某个对象

```
#region 方法1，判断是否继承了某个接口或者类
Type t = typeof(IActionForLicenseCheck);
Type tt = typeof(TestForCheck);
Debug.Log("方法1:"+t.IsAssignableFrom(tt));
#endregion

#region 方法2，判断是否继承了某个接口
Debug.Log("方法2:" + obj.GetType().GetInterfaces().Contains(typeof(IActionForLicenseCheck)));
#endregion
```

### * 截屏

```
Application.CaptureScreenshot() // 已废弃
ScreenCapture.CaptureScreenshot(savePath) // 推荐
```

### * 禁止息屏

```
Screen.sleepTimeout = SleepTimeout.NeverSleep;
```

### * 区分编辑器模式

```
// 宏定义
#if UNITY_EDITOR
  UnityEngine.Debug.Log(message);
#endif

// 特性标记
[Conditional("UNITY_EDITOR")]
public static void Log(object message)
{
    UnityEngine.Debug.Log(message);
}
```

### * 类似王者荣耀中画面质量修改：低、中、高

```
// 打印所有支持质量
for (int i = 0; i < QualitySettings.names.Length; i++)
{
	// 如：Very Low，Low，Medium，High，Very Heigh
	Debug.Log(QualitySettings.names[i].ToString());
}
// 设置新画面质量
QualitySettings.SetQualityLevel(0, true); 
Debug.Log("当前质量:" + QualitySettings.GetQualityLevel().ToString());
```

### * 代码设置分辨率

```
Screen.SetResolution(width, height, true);
```

### * 注释插入链接

```
/// <footer><a href="https://docs.unity3d.com/2020.3/Documentation/ScriptReference/30_search.html?q=QualitySettings">`QualitySettings` on docs.unity3d.com</a></footer>
```

### * 判断是否点击UI上

```
#if IPHONE || ANDROID
	if (EventSystem.current.IsPointerOverGameObject(Input.GetTouch(0).fingerId))
#else
	if (EventSystem.current.IsPointerOverGameObject())
#endif
		Debug.Log("当前触摸在UI上");
	else 
		Debug.Log("当前没有触摸在UI上");
```

### * PlayerPrefs

Unity3d持久化类——PlayerPrefs，以键值对的形式将数据保存在文件中

1、PlayerPrefs类支持3种数据类型的保存和读取：浮点型（Float）、整形（Int）、字符串型（String）   
PlayerPrefs.SetInt()、PlayerPrefs.GetInt();   
PlayerPrefs.SetFloat()、PlayerPrefs.GetFloat();   
PlayerPrefs.SetString()、PlayerPrefs.GetString();

2、提供的其他方法：   
PlayerPrefs.DeleteKey(key:string)；删除指定的数据    
PlayerPrefs.DeleteAll()；删除全部键；   
PlayerPrefs.Haskey(key:string)；判断数据是否存在的方法，bool   

### * 字符串特殊格式化输出

```
Debug.Log($"货币: {100:C} {100:c}"); // 不确定货币符号，谨慎使用
Debug.Log($"十进制: {100:D4} {1:d4}"); // 不足4位用0填充 0001 [够4位该显示什么就显示什么]
Debug.Log($"科学计数法: {100:E} {1000000:e}");
Debug.Log($"固定点: {100.4:F2} {100.4:f2}"); // 保留两位小数，不足就0填充 [100.456显示100.45  100.4显示100.40]
Debug.Log($"百分数: {1:P} {1:p}");
Debug.Log($"十六进制: {100:X} {100:x}");
```

### * Addressables工程分离

需要把另外工程打包的catalog.json文件和bundle文件放在服务器的地址里面。

Addressable是支持额外加载catalog的，需要做好路径匹配，调用Addressables.LoadContentCatalogAsync(extraCatalogPath)，加载完之后，就可以正常加载extraCatalogPath对应的路径下的资源的，这里的extraCatalog可以是另外的工程里面创建的catalog.json和对应的bundle资源。
[UWA 问答](https://answer.uwa4d.com/question/5e80b40f86713137aaa0991f)


### * 高亮显示Debug.Log对应的游戏对象

当使用Debug.Log方法输出信息时，可将gameObject作为此方法的第二个参数，当程序运行时，点击Console面板中对应的输出信息，可在Hierarchy面板中高亮显示挂载了此脚本的游戏对象

```
Debug.Log("点击该句在Hierarchy面板高亮挂载此脚本的对象", gameObject);
Debug.Log("点击该句在Hierarchy面板高亮挂载此脚本的对象", _text.gameObject);
```

### * Inspector调试模式

在Inspector面板右上角下拉菜单选择Debug启动调试模式，将显示组件的所有变量，包括私有变量。

### * 正式环境关闭日志

1. Release环境只打印 error或者不打印（Debug.Log会产生GC，影响性能）

```
if (Debug.isDebugBuild) {
    Debug.unityLogger.logEnabled = true;
    //Debug.unityLogger.filterLogType = LogType.Log; //显示所有
} else {
    Debug.unityLogger.logEnabled = false;
    //Debug.unityLogger.filterLogType = LogType.Error;/／只显示Error + Exception
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

### * 不挂载加载而执行方法

```
[RuntimeInitializeOnLoadMethod]
public static void Start()
{
    Application.logMessageReceivedThreaded += OnLogCallback;
}
```

### * Scripting Define Symbols (宏定义)

```
在Unity Player(project) Settings -> Player -> Scripting Define Symbol 添加LOG_ON

使用方式1
[Conditional("LOG_ON")]
public static void Log(this object obj, object message, UnityEngine.Object context = null) {
    UnityEngine.Debug.Log(message, context);
}

使用方式2
#if UNITY_EDITOR
	Debug.Log("我只有在编辑器环境下才会被编译和执行");
#endif

通过代码获取定义符
var symbols = PlayerSettings.GetScriptingDefineSymbolsForGroup(BuildTargetGroup.Android);

通过代码设置定义符
PlayerSettings.SetScriptingDefineSymbolsForGroup(BuildTargetGroup.Android, "MY_DEFINE");
```

### * 获取当前函数、行号等

```
string callStack = "CallStack:[" + new System.Diagnostics.StackTrace().GetFrame(1).GetMethod() + "] => ";
string FileName = " FileName:[" + System.IO.Path.GetFileName(new System.Diagnostics.StackTrace(1, true).GetFrame(0).GetFileName()) + "]";
string LineNumber = " Line:[" + new System.Diagnostics.StackTrace(1, true).GetFrame(0).GetFileLineNumber().ToString() + "]";
string sceneName = " Scene:[" + SceneManager.GetActiveScene().name + "]";
string gameObjectName = " GameObject:[" + go.name + "]";
string className = " Class:[" + System.Reflection.MethodBase.GetCurrentMethod().ReflectedType.FullName + "]";
string functionName = " Function:[" + System.Reflection.MethodBase.GetCurrentMethod().Name + "]";
string log = " Log:[" + "GetComponentNullLog" + "]";
string OutputResult = callStack + FileName + LineNumber + sceneName + gameObjectName + className + functionName + log;
Debug.Log(OutputResult);
```
[参考](https://blog.csdn.net/u014361280/article/details/103946166)

### 网络延时

```
using UnityEngine;
 
namespace Utils
{
    public class PingUtil : MonoBehaviour
    {
        public string ip = string.Empty;
        Ping ping;
        string label;
        GUIStyle guiStyle;
 
        void Start()
        {
            ip = "121.42.114.17";    // 这里我用的是我网站的ip(www.u3d8.com)  需要替换成自己的服务器ip
 
            SendPing();
 
            guiStyle = new GUIStyle();
            guiStyle.normal.background = null;
            guiStyle.fontSize = 40;
 
        }
 
        bool isNetWorkLose = false;
        void OnGUI()
        {
            if (Application.internetReachability == NetworkReachability.NotReachable)
            {
                label = "460";
                SetColor(460);
                isNetWorkLose = true;
            }
            else if (isNetWorkLose || (null != ping && ping.isDone))
            {
                isNetWorkLose = false;
                label = ping.time.ToString();
                SetColor(ping.time);
                ping.DestroyPing();
                ping = null;
                Invoke("SendPing", 1);//每秒Ping一次
            }
 
            GUI.Label(new Rect(10, 50, 200, 50), "ping:" + label + "ms", guiStyle);
        }
        void SendPing()
        {
            ping = new Ping(ip);
        }
 
        /// <summary>
        /// 仿王者荣耀延迟过高，颜色变化
        /// </summary>
        /// <param name="pingValue"></param>
        void SetColor(int pingValue)
        {
            if (pingValue < 100)
            {
                guiStyle.normal.textColor = new Color(0, 1, 0);
            }
            else if (pingValue < 200)
            {
                guiStyle.normal.textColor = new Color(1, 1, 0);
            }
            else
            {
                guiStyle.normal.textColor = new Color(1, 0, 0);
            }
        }
    }
}
```

### Unity path 在移动端目录

```
// iOS
Application.dataPath : Application/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/xxx.app/Data
Application.streamingAssetsPath : Application/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/xxx.app/Data/Raw
Application.persistentDataPath : Application/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/Documents
Application.temporaryCachePath : Application/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/Library/Caches

// Android
Application.dataPath : /data/app/xxx.xxx.xxx.apk
Application.streamingAssetsPath : jar:file:///data/app/xxx.xxx.xxx.apk/!/assets
Application.persistentDataPath : /data/data/xxx.xxx.xxx/files
Application.temporaryCachePath : /data/data/xxx.xxx.xxx/cache
```

* [Unity3D 判断脚本是否继承了某个接口或者类](https://www.jianshu.com/p/17c648673c4b)
* [100条Unity基础小贴士，从新手成为熟手](https://bbs.gameres.com/forum.php?mod=viewthread&tid=837368)
* [Unity的RuntimeInitializeOnLoadMethod属性初探](https://www.cnblogs.com/meteoric_cry/p/7602122.html)