> ### Addressable Asset System 简介

Addressable Asset System 可寻址资源系统AAS，通过地址轻松加载资源，它简化了资源包创建和部署。AAS通过异步加载支持从任何位置加载任何依赖项集合。无论可寻址资产是在本地还是网络上。可以通过地址加载单个可寻址资产，也可以使用自定义组标签加载许多可寻址资产。  

Addressable Asset System 有两部分组成
* Addressable Assets Package(核心部分)
* Scriptable Build Pipline Package (依赖包)

当安装Addressable Assets Package后，Package Manager也会自动安装Scriptable Build Pipline Package

> ### 为什么要使用Addressable Asset System

传统的资源组织方式让游戏在资源加载上具有很大挑战

* 迭代时间

```
通过地址引用资源很高效，对资源优化不再影响代码
```

* 资源依赖关系管理

```
系统不仅返回地址的资产内容，还返回它所有的依赖项关系，所有的meshes(网格)、shaders(着色器)、animations(动画)等都会被加载。
```

* 内存管理

```
资源加载和卸载都很简单，引用自动计数，强大的分析器(profiler)可快速找到内存问题
```

* 资源打包

```
因为AAS处理好了资源映射和复杂的依赖关系，所以打包bundles(捆绑包)很高效，即使移动资源位置或重命名，也可轻松部署资源到本地或远端，支持维护下载内容(DLC)和减少程序大小
```

* 相关配置

```
AAS可以在统一的配置中，创建和设置一些属性来决定资源如何被打包
```

> ### Addressable Asset System 和 AssetBundle区别

AAS是由Addressables和ResourceManager组成的系统

* Addressables和 ResourceManager位于不同的包
* Addressables依赖于 ResourceManager
* 在PackageManager中引入Addressables时，ResourceManager会自动包含
* 全部用C#编写

ResourceManager是一个用于正确管理资源加载和卸载的框架

* 使用引用计数执行卸载管理
* DI可以定制记载过程
* 具有方便的功能，如显示资源加载情况的分析器

AAS与 AssetBundle区别

1. 可寻址资源系统只需要一个资源的地址就可以从任意地方加载，而AssetBundle需要从定制的Bundle中加载
2. AAS加载到内存中的bundle有引用计数，而AssetBundle加载到内存中的bundle需要自己管理
3. 可寻址资源系统会自己管理依赖关系，而AssetBundle需要自己管理依赖关系，维护较为困难
4. AAS默认加载操作为异步操作，可以添加事件监听，而AssetBundle则有同步和异步加载

> ### 一些名词概念

**Address**: 一个资源(asset)的索引标识，运行时可通过它找到指定资源
**AddressableAssetData**:该目录在项目的Assets目录下，包含所有的Addressable Assets数据和其他相关的配置
**Asset group**:代表一组可在构建时被打包的可寻址资源
**Asset group schema**:项目里每一个Asset group对应的配置，在资源构建时会被引用到
**AssetReference**:它像引用(reference)一样，但是通常不会被立即初始化，AssetReference对象存储了资源的GUID，需要的时候可通过它来加载资源
**Asynchronous loading** [eɪˈsɪŋkrənəs]异步加载，AAS的基础，在开发过程可任意修改资源和依赖资源的位置而不需要修改任何代码
**Build Script**:打包时会按照它所设定步骤来打包资源，同时也为Resource Manager提供Addressables和资源位置的引用关系
**Label**:提供额外的资源索引标识，可在运行时加载相同类别的资源 ``` 如 Addressables.DownloadDependeciesAsync("spaceHazards") ```

> ### 使用

#### 安装Addressable Assets

Window -> Package Manager -> Unity Registry -> Addressables -> Install

#### 添加Addressables Groups

Window -> Asset Management -> Addressables -> Groups

默认两组 Build In Data，Default Local Group

#### 设置资源为Addressable

Unity编辑器中有两种方式可以设置资源为Addressable    
1.在资源的Inspectoer，2.在Addressables Groups窗口(将资源拖入)

#### 指定资源索引标识(address)

在Addressables Groups窗口，选中资源，再单击或者右击选Change Address 来修改资源标识。使用Addressables时，系统会把编辑时和运行时相关配置和数据都保存到Assets/AddressableAssetsData目录下的文件中，所以此目录也需要提交到版本管理。

#### 构建打包资源

构建游戏前，需要把资源打包到bundle中，打包有两种方式

1. Addressables Groups中选择Build -> New Build -> Default Build Script
2. 使用API，[AddressableAssetSettings.BuildPlayerContent()](https://docs.unity3d.com/Packages/com.unity.addressables@1.3/api/UnityEditor.AddressableAssets.Settings.AddressableAssetSettings.html#UnityEditor_AddressableAssets_Settings_AddressableAssetSettings_BuildPlayerContent)

#### 通过address加载或实例化

你可以在运行时加载或实例化一个资源，当加载一个资源时，系统会自动加载所有依赖资源到内存(包括资源上的捆绑数据)，但要注意，在没有实例化前，加载的资源并不会出现在场景中。

```C#
// This loads the asset with the specified address
Addressables.LoadAssetAsync<GameObject>("AssetAddress");

// This instantiates the asset with the specified address into your Scene.
Addressables.InstantiateAsync("AssetAddress");
```

**注意**: `LoadAssetAsync`和`InstantiateAsync`都是异步操作，你需要提供一个回调，他会在资源完成后被调用

```
void Start()
{
	hand = Addressables.LoadAssetAsync<GameObject>("Cube");
	hand.Completed += OnResLoadedHandler;
}
​
private void OnResLoadedHandler(AsyncOperationHandle<GameObject> obj)
{
	GameObject go = obj.Result;
	Instantiate(go);
}
```

#### 通过AssetReference加载

AssetReference 资源引用类提供了一个机制来访问资源，而不需要知道具体字符串地址。 

任何可序列化component都支持AssetReference变量(MonoBehaviour脚本，ScriptableObject，其他可序列化类)，在脚本中添加一个成员 public AssetReference assetReference，在Inspector检视视图中就可以绑定可寻址资产了。

```
[SerializeField] private AssetReference _assetReference;
// 实例化对象
_assetReference.LoadAssetAsync<GameObject>();
_assetReference.InstantiateAsync(pos, rot);
// 销毁对象
if (!Addressables.ReleaseInstance(gameObject)) Destroy(gameObject);
// 对象将被Addressables.ReleaseInstance销毁，即使不是以这种方式创建
```

#### 子资源和components

在**Addressables Groups**窗口，所有的资源名称都能看到，包括子资源

如果想加载子资源，只需指定一些特殊的加载参数。官方从图集加载图片与从FBX文件加载动画的示例[sprite loading](https://github.com/Unity-Technologies/Addressables-Sample/tree/master/Basic/Sprite Land)   

加载GameObject上的component需要加载实例化得到gameObject后才能获得component的引用，而不是通过Addressables直接取加载GameObject上的component。官方示例[ComponentReference](https://github.com/Unity-Technologies/Addressables-Sample/tree/master/Basic/ComponentReference)

如果一个包含子资源的资源(如SpriteAtlas或FBX)添加到AssetReference，你可以选择资源本身或选择对应子资源。在属性后面会出现两个下拉框，第一个是选择资源本身，第二个是选择子资源。如果第二个下拉框选择 none 则代表使用默认主资源。   

> ### 打包apk/ipa

* **LocalBuildPath**:打包后保存的路径
* **LocalLoadPath**:本地加载路径，发布后将自动放到StreamingAssets里
* **RemoteBuildPath**:热更新打包保存路径，一般在工程的ServerData文件夹里
* **RemoteLoadPath**:远程加载路径，格式http(s)://xxx/[buildTarget]
* **Labels**:设置加载的组，当加载地址后将会整个label的内容都下载下来

```C#
// 获取default组bundle大小，也可以单独设置预制体标签下载
Addressables.GetDownloadSizeAsync("default");
// 预下载default组，下载后缓存到本地
Addressables.DownloadDependenciesAsync("default");
```

项目打全包时，先选择为LocalBuildPath和LocalLoadPath，这样aa包会包含进apk或ipa中，然后打热更包时，则使用RemoteBuildPath和RemoteLoadPath，并要设置为Can Change Post Release，这样就可以了，既能把资源包含进apk，同时也能增量更新

> ### 动态设置远程路径变量

```
public static void SetProfileSettingPath()
{
    string remoteBuildPath = "ServerData/[BuildTarget]";
    string remoteLoadPath = "http://localhost/[BuildTarget]";
 
    AddressableAssetSettings mSettings = AddressableAssetSettingsDefaultObject.Settings;
    string profileId = mSettings.activeProfileId; //mSettings.profileSettings.GetProfileId("Dynamic");
    mSettings.profileSettings.SetValue(profileId, AddressableAssetSettings.kRemoteBuildPath, remoteBuildPath);
    mSettings.profileSettings.SetValue(profileId, AddressableAssetSettings.kRemoteLoadPath, remoteLoadPath);
    Debug.Log($"设置Addressable Groups Profile完成");
    Debug.Log($"{AddressableAssetSettings.kRemoteBuildPath}:{remoteBuildPath}");
    Debug.Log($"{AddressableAssetSettings.kRemoteLoadPath}:{remoteLoadPath}");
}
```

* [学习Unity新出资源管理系统-Addressable Assets](https://www.jianshu.com/p/e79b2eef97bf)
* [Addressable研究](https://blog.csdn.net/asmdajksdjdsa/article/details/99679619)
* [Addressable Asset System 中文文档](https://github.com/xiexx-game/addressable-asset-system-chinese-manual)
* [Unity官方案例-Addressables-Sample-笔记(1)-Basic AssetReference(基本加载)](https://blog.csdn.net/kill566666/article/details/119001385)
* [Addressables.CN 系统资源加密](https://zhuanlan.zhihu.com/p/359766894)
* [Addressables学习笔记3: 实际操作实现资源热更新](https://www.cnblogs.com/thpGames/p/10361949.html)
* [Unity Addressable发布至服务器流程与自动打包代码](https://blog.csdn.net/u011366226/article/details/104506802/)