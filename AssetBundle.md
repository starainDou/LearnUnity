> ### AssetBundle 简介

Assetbundle是特定平台的资产压缩文件，包括：模型、贴图、预制件、视频、音频等。但除了要执行的.cs脚本文件，因为它是编译型文件，所以要通过Lua或者ILRuntime来进行热更新。

AssetBundle的好处：
 相对于Resources(本地资源)，AB包更好管理资源。Resources文件夹下所有东西都会被打包，不管有没有用，且打包后只读。但AB包通过处理可读可写，包储存位置可自定义，压缩方式有可选性，最大的好处就是可动态更新它。压缩资源形式，外加后期下载更新方式，大大减少了原始包体积。
 
[官方AssetBundles-Browser](https://github.com/Unity-Technologies/AssetBundles-Browser.git)，其中
Compression压缩方式：
1. NoCompression 不压缩，包较大，不推荐
2. LZMA 压缩后最小，但解压慢，且用一个资源要解压所有
3. LZ4 压缩后相对LZMA大一点，但用什么解压什么，内存占用低

* Exclude Type Infomation: 在资源包中不包含的资源的类型信息
* Force Rebuild 强制重新构建[与ClearFolders不同，它不删除不再存在的包]
* Ignore Type Tress Changes 增量构建检查时忽略类型树的改变
* Append Hash 将哈希值附加到资源名上 
* Strict Mode 严格模式[如果打包时报错则打包失败]
* Dry Run Build 运行时构建

> ### 加载

同步加载

```
/// <summary>同步加载</summary>
private void LoadABResource(string path, string abName, string resName)
{
    AssetBundle ab = AssetBundle.LoadFromFile(path + "/" + abName);
    // 建议使用泛型或Type反射进行加载
    // Type
    // GameObject obj1 = ab.LoadAsset(resName, typeof(GameObject)) as GameObject;
    // Instantiate(obj1);
    
    // 泛型
    GameObject obj2 = ab.LoadAsset<GameObject>(resName);
    Instantiate(obj2);
    
    // 卸载 false:只卸载AB包集，但是不卸载资源
    // AssetBundle.UnloadAllAssetBundles(false); // 全部卸载
    // ab.Unload(false); // true则显示成分红占位
}
// 同名AB包不能同时加载
```

 异步加载
 
 ```
 // StartCoroutine(LoadABResourceAsync(Application.streamingAssetsPath, "abtest", "ABTestObject"));
 
private IEnumerator LoadABResourceAsync(string path, string abName, string resName)
{
    yield return null;
    // 加载资源
    AssetBundleCreateRequest abCreateRequest = AssetBundle.LoadFromFileAsync(path + "/" + abName);
    // 加载完成
    yield return abCreateRequest;
    AssetBundleRequest abRequest = abCreateRequest.assetBundle.LoadAssetAsync(resName, typeof(GameObject));
    yield return abRequest;
    GameObject res = abRequest.asset as GameObject;
    Instantiate(res);
}
 ```
 
 > ### 参考文章
 
 * [AssetBundle生成AB文件](https://blog.csdn.net/zhanxxiao/article/details/113732922)
 * [Unity资源打包（二）：AssetBundle工具制作-2020更新](https://blog.csdn.net/leichao168/article/details/108850534)
 * [官方AssetBundles-Browser](https://github.com/Unity-Technologies/AssetBundles-Browser.git)
 * [关于AB打包的详解](https://blog.csdn.net/jjl1991_11/article/details/80118765)
 * [Unity AssetBundle 基础操作](https://blog.csdn.net/weixin_43925843/article/details/119817409)
 * [Unity资源打包学习笔记（一）、详解AssetBundle的流程](https://www.cnblogs.com/zblade/p/9198647.html)