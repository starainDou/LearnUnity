CalliOS.cs

```
public class CalliOS : MonoBehaviour
{
    public Button buttonInit;
    public Button buttonInter;
    public Button buttonReward;
    public Button buttonEnter;
    public Button buttonLeave;
    
#if UNITY_IOS || UNITY_IPHONE
    #region Unity 调用 iOS
    [DllImport("__Internal")] 
    private static extern void initSDK();
    
    [DllImport("__Internal")] 
    private static extern string getInfoWithNumber(int number);
    #endregion
    
    #region iOS 调用 Unity
    [UnmanagedFunctionPointer(CallingConvention.Cdecl)]
    public delegate string callback_delegate(int val);
    
    [DllImport("__Internal")]
    private static extern void CalliOSRegister(callback_delegate callback);
    
    [AOT.MonoPInvokeCallback(typeof(callback_delegate))] // using AOT;
    private static string cs_callback(int val) {
        //Debug.Log ("cs_callback : " + val);
        Console.WriteLine("Unity cs_callback : " + val);
        return $"我是Unity返回值{val}";
    }
    
    [DllImport("__Internal")] 
    private static extern void iosCallUnityPointerCmd();

    // 不能带有返回值；必须要挂载到对象后才能调用  需要有固定的游戏物体，按名称查找，并且是激活状态，不能隐藏
    void iOSCallUnitySendMessage(string message)
    {
        Console.WriteLine("Unity iOSCallUnitySendMessage : " + message);
        // Debug.Log ("iOSCallUnitySendMessage : " + message);
    }
    
    [DllImport("__Internal")] 
    private static extern void iosCallUnitySendMessageCmd();
    #endregion
#endif
    
    
    
    void Start()
    {
        bindingEvent();
    }

    // Update is called once per frame
    void Update()
    {
        
    }
    
             
    [RuntimeInitializeOnLoadMethod(RuntimeInitializeLoadType.AfterSceneLoad)]
    public static void DoSomething()
    {
        Debug.Log("It's the start of the game 不挂载自己执行");
    }


    private void bindingEvent()
    {
        buttonInit.onClick.AddListener(() =>
        {
            initSDK();
        });
        buttonInter.onClick.AddListener(() =>
        {
            string message = getInfoWithNumber(10086);
            Console.WriteLine("Unity UnityCalliOS return : " + message);
            //Debug.Log ("UnityCalliOS return : " + message);
        });
        buttonReward.onClick.AddListener(() =>
        {
            CalliOSRegister(cs_callback);
        });
        buttonEnter.onClick.AddListener(() =>
        {
            iosCallUnityPointerCmd();
        });
        buttonLeave.onClick.AddListener(() =>
        {
            iosCallUnitySendMessageCmd();
        });
    }
}

```

CalliOS.h

```
#import <Foundation/Foundation.h>

@interface CalliOS : NSObject

#if defined (__cplusplus)
extern "C" {
#endif
    
    void initSDK();
    
    const char * getInfoWithNumber(int number);
    const char* makeStringCopy(const char* string);
    
    typedef const char * _Nullable (*cs_callback)(int);
    void CalliOSRegister(cs_callback callback);
    
    void iosCallUnityPointerCmd();
    
    void iosCallUnitySendMessageCmd();
    
#if defined (__cplusplus)
}
#endif

@end
```

CalliOS.mm

```
#import "CalliOS.h"
#import "Interaction.h"

@implementation CalliOS

#if defined (__cplusplus)
extern "C" {
#endif
    
    void initSDK() {
        [Interaction initSDK];
    }
    
    const char * getInfoWithNumber(int number) {
        int tempNum = number;
        // strdup(const char *__s1) 复制mHost字符串,通过Malloc()进行空间分配 
        // return strdup(mHost);
        return makeStringCopy([[Interaction getInfoWithNumber:tempNum] UTF8String]);
    }
    
    const char* makeStringCopy(const char* string) {
      if (NULL == string) {
        return NULL;
      }
      char* res = (char*)malloc(strlen(string)+1);
      strcpy(res, string);
      return res;
    }
    
    static cs_callback callbackBlock;
    void CalliOSRegister(cs_callback callback) {
        callbackBlock = callback;
    }
    
    void iosCallUnityPointerCmd() {
        const char * cStr = callbackBlock(10011);
        NSLog(@"iOS %s %@", cStr, [[NSString alloc] initWithUTF8String:cStr]);
        // 通过将self指针桥接为oc 对象来调用oc方法
    }
    
    void iosCallUnitySendMessageCmd() {
        UnitySendMessage("Main Camera", "iOSCallUnitySendMessage", @"iosCallUnitySendMessageCmd".UTF8String);
    }
    
#if defined (__cplusplus)
}
#endif

@end
```

Interaction

```
#import <Foundation/Foundation.h>

NS_ASSUME_NONNULL_BEGIN

@interface Interaction : NSObject

+ (void)initSDK;

+ (NSString *)getInfoWithNumber:(NSInteger)number;

@end

NS_ASSUME_NONNULL_END



#import "Interaction.h"

@implementation Interaction

+ (void)initSDK {
    NSLog(@"iOS %@", NSStringFromSelector(_cmd));
}

+ (NSString *)getInfoWithNumber:(NSInteger)number {
    NSLog(@"iOS %@", NSStringFromSelector(_cmd));
    if (number > 10) {
        return [NSString stringWithFormat:@"大于10：%ld", number];
    } else {
        return [NSString stringWithFormat:@"小于10：%ld", number];
    }
}

@end

```

修改工程和plist

```
using UnityEngine;
using UnityEditor;
using UnityEditor.Callbacks;
using System;
#if UNITY_EDITOR_OSX
using UnityEditor.iOS.Xcode;
#endif
using System.IO;

public class XcodeProjectMod : MonoBehaviour
{
#if UNITY_EDITOR_OSX
    [PostProcessBuild]
    public static void OnPostprocessBuild(BuildTarget buildTarget, string path)
    {
        if (buildTarget == BuildTarget.iOS)
        {
            string projPath = PBXProject.GetPBXProjectPath(path);
            PBXProject proj = new PBXProject();

            proj.ReadFromString(File.ReadAllText(projPath));
            string target = proj.TargetGuidByName("Unity-iPhone");
            string rootPath = projPath.Replace("Unity-iPhone.xcodeproj/project.pbxproj", "");

            //复制sdk接口文件
            var iosLibPath = "Sdk/IOSLib";
            var classes = "Classes";
            var fileName = "UnityAppController.mm";
            var filePath = Path.Combine(iosLibPath, fileName);
            var targetFilePath = Path.Combine(path, Path.Combine(classes, fileName));
            File.Copy(filePath, targetFilePath, true);

            fileName = "UnityAppController.h";
            filePath = Path.Combine(iosLibPath, fileName);
            targetFilePath = Path.Combine(path, Path.Combine(classes, fileName));
            File.Copy(filePath, targetFilePath, true);

            //创建swift桥接
            fileName = "File.swift";
            filePath = Path.Combine(iosLibPath, fileName);
            targetFilePath = Path.Combine(path, fileName);
            File.Copy(filePath, targetFilePath, true);
            proj.AddFileToBuild(target, proj.AddFile(fileName, fileName, PBXSourceTree.Source));

            fileName = "Unity-iPhone-Bridging-Header.h";
            filePath = Path.Combine(iosLibPath, fileName);
            targetFilePath = Path.Combine(path, fileName);
            File.Copy(filePath, targetFilePath, true);
            proj.AddFileToBuild(target, proj.AddFile(fileName, fileName, PBXSourceTree.Source));

            //复制sdk文件到xcode目录下
            var sdkPath = Application.dataPath.Replace("Assets", "") + "Sdk/IOSLib/SDK";
            var sdkFolder = "SDK";
            var sdkDir = Path.Combine(path, sdkFolder);
            CopyOldLabFilesToNewLab(sdkPath, sdkDir);
            //以Group方式添加文件夹引用到工程
            //AddResourceGroupToiOSProject(path, proj, target, sdkFolder);

            // 添加依赖库
            proj.AddFrameworkToProject(target, "AdServices.framework", true);
            proj.AddFrameworkToProject(target, "AppTrackingTransparency.framework", false);
            proj.AddFrameworkToProject(target, "AppsFlyerLib.framework", false);
            proj.AddFrameworkToProject(target, "Accounts.framework", false);

            proj.SetBuildProperty(target, "ENABLE_BITCODE", "NO");
            proj.SetBuildProperty(target, "SWIFT_VERSION", "5.0");

            //设置包名
            proj.SetBuildProperty(target, "PRODUCT_BUNDLE_IDENTIFIER", "com.baplay.${PRODUCT_NAME:rfc1034identifier}");
            proj.SetBuildProperty(target, "PRODUCT_NAME", "st");

            //添加链接库
            string linkerFlags = "$(inherited) -ObjC -lsqlite3.0 -lc++ -lstdc++ -lz -weak_framework Accelerate -weak-lSystem -weak_framework CoreMotion -weak-lSystem";
            proj.SetBuildProperty(target, "OTHER_LDFLAGS", linkerFlags);
            //string[] linkerFlagsToAdd = { "-lsqlite3.0", "-ObjC", "-lc++", "-lstdc++", "-lz", "-weak_framework", "Accelerate", "-weak-lSystem", "-weak_framework", "CoreMotion", "-weak-lSystem" };
            //string[] linkerFlagsRemove = null;//删除的链接库
            //proj.UpdateBuildProperty(target, "OTHER_LDFLAGS", linkerFlagsToAdd, linkerFlagsRemove);

            string[] codeSignEntitlements = { "$(PROJECT_DIR)/SDK/Resource/Play.entitlements" };
            proj.UpdateBuildProperty(target, "CODE_SIGN_ENTITLEMENTS", codeSignEntitlements, null);

            //添加库引用路径
            string[] FrameworkSearchPaths = { "$(PROJECT_DIR)/SDK/OtherLibs",
                    "$(PROJECT_DIR)/SDK/OtherLibs/GoogleSignIn_5.0.2",
                    "$(PROJECT_DIR)/SDK/OtherLibs/AF-iOS-SDK_v5.1" };
            proj.UpdateBuildProperty(target, "FRAMEWORK_SEARCH_PATHS", FrameworkSearchPaths, null);

            //添加资源引用路径
            string[] LibrarySearchPaths = { "$(PROJECT_DIR)/SDK/OtherLibs/inMobi_5.0.1",
                    "$(PROJECT_DIR)/SDK/OtherLibs/OL113Tool",
                    "$(PROJECT_DIR)/SDK" };
            proj.UpdateBuildProperty(target, "LIBRARY_SEARCH_PATHS", LibrarySearchPaths, null);

            File.WriteAllText(projPath, proj.WriteToString());

            // 修改Info.plist文件
            var plistPath = Path.Combine(rootPath, "Info.plist");
            var plist = new PlistDocument();
            plist.ReadFromFile(plistPath);

            //增加权限
            plist.root.SetString("NSPhotoLibraryUsageDescription", "需要您的同意,才能访问相册");//
            plist.root.SetString("NSMicrophoneUsageDescription", "需要您的同意,才能访问麦克风");
            plist.root.SetString("NSCameraUsageDescription", "需要您的同意,才能访问相机");
            plist.root.SetString("NSTrackingUsageDescription", "您同意App使用IDFA廣告識別碼來做個人最佳化廣告嗎?");

            //增加键值对
            plist.root.SetString("bundle id", "com.test.test");
            plist.root.SetString("FacebookAppID", "819");
            var schemes = plist.root.CreateArray("LSApplicationQueriesSchemes");
            schemes.AddString("line");
            schemes.AddString("fbapi");
            schemes.AddString("fb-messenger-api");
            schemes.AddString("fbauth2");
            schemes.AddString("fbshareextension");

            // 插入URL Scheme到Info.plsit（理清结构）
            PlistElementArray urlArray = null;
            if (!plist.root.values.ContainsKey("CFBundleURLTypes"))
                urlArray = plist.root.CreateArray("CFBundleURLTypes");
            else
                urlArray = plist.root.values["CFBundleURLTypes"].AsArray();
            var dict = urlArray.AddDict().CreateArray("CFBundleURLSchemes");
            //插入dict
            dict.AddString("stios");
            dict.AddString("fb124442819");

            // 应用修改
            plist.WriteToFile(plistPath);
        }
    }
    /// <summary>
    /// 拷贝文件夹下所有文件到新目录下面
    /// </summary>
    /// <param name="sourcePath">l文件所在目录</param>
    /// <param name="savePath">保存的目标目录</param>
    /// <returns>返回:true-拷贝成功;false:拷贝失败</returns>
    public static bool CopyOldLabFilesToNewLab(string sourcePath, string savePath)
    {
        if (!Directory.Exists(savePath))
        {
            Directory.CreateDirectory(savePath);
        }

        try
        {
            string[] labDirs = Directory.GetDirectories(sourcePath);//目录
            string[] labFiles = Directory.GetFiles(sourcePath);//文件
            if (labFiles.Length > 0)
            {
                for (int i = 0; i < labFiles.Length; i++)
                {
                    if (!labFiles[i].Contains(".meta") && !labFiles[i].Contains(".DS_Store"))//排除文件
                    {
                        string fileName = Path.GetFileName(labFiles[i]);
                        File.Copy(Path.Combine(sourcePath, fileName), Path.Combine(savePath, fileName), true);
                    }
                }
            }
            if (labDirs.Length > 0)
            {
                for (int j = 0; j < labDirs.Length; j++)
                {
                    string fileName = Path.GetFileName(labDirs[j]);
                    Directory.GetDirectories(Path.Combine(sourcePath, fileName));

                    //递归调用
                    CopyOldLabFilesToNewLab(Path.Combine(sourcePath, fileName), Path.Combine(savePath, fileName));
                }
            }
        }
        catch (Exception)
        {
            return false;
        }
        return true;
    }
    //在工程文件添加资源文件夹引用，以 Create Group 形式加到工程文件
    private static void AddResourceGroupToiOSProject(string xcodePath, PBXProject proj, string target, string resourceDirectoryPath)
    {
        string dirFullPath = Path.Combine(xcodePath, resourceDirectoryPath);
        //添加文件引用
        string[] files = Directory.GetFiles(dirFullPath);
        foreach (string s in files)
        {
            string fileExtension = Path.GetExtension(s);
            if (fileExtension.Equals(".DS_Store"))
            {
                continue;
            }
            string fileName = Path.GetFileName(s);
            string targetFilePath = Path.Combine(resourceDirectoryPath, fileName);
            string fileGuid = proj.AddFolderReference(targetFilePath, targetFilePath);
            proj.AddFileToBuild(target, fileGuid);
        }
        //添加子文件夹
        string[] subDirectorys = Directory.GetDirectories(dirFullPath);
        foreach (string s in subDirectorys)
        {
            string fileName = Path.GetFileName(s);
            string subDirPath = Path.Combine(resourceDirectoryPath, fileName);
            string fileExtension = Path.GetExtension(s);
            //mac os文件结构原因需要处理文件后缀，不然会变成文件夹，导致库无法正常识别
            if (fileExtension.Equals(".xcassets")
                || fileExtension.EndsWith(".framework")
                || fileExtension.EndsWith(".xcframework")
                || fileExtension.EndsWith(".bundle")
                || fileExtension.EndsWith(".strings"))
            {
                string fileGuid = proj.AddFile(subDirPath, subDirPath);
                proj.AddFileToBuild(target, fileGuid);
            }
            else
            {
                AddResourceGroupToiOSProject(xcodePath, proj, target, subDirPath);
            }
        }
    }
#endif
}
```

* [Dynamic Library 的 framework 添加 Embedded Binaries](https://blog.csdn.net/zbfaaadjl/article/details/79291513)
* [facebook unity DisableBitcode.cs](https://github.com/facebook/facebook-sdk-for-unity/blob/def5dbea64f8466ad7e1aa046e5cfdd319adeb38/UnitySDK/Assets/Editor/DisableBitcode.cs#L29)
* [Unity自动化Xcode配置及接入SDK踩坑记录](https://blog.csdn.net/KindSuper_liu/article/details/122984131)
* [iOS项目嵌入Unity3D](https://juejin.cn/post/7054059279841656868)
* [浅谈CocoaPods如何在Unity中直接配置](https://zhuanlan.zhihu.com/p/228230154)
* [CocoaPods 接入 KSAdSDK](https://www.cnblogs.com/sylvan/p/15313618.html)
* [修改工程配置、修改plist](https://www.jianshu.com/p/8471114d6c3d)
* [Unity gitIgnore](https://github.com/github/gitignore/blob/main/Unity.gitignore)
* [如何在unity中通过pod来配置SDK](wordpress.tradplusad.com/?docs=doc_u3d/u3d_advanced_features/ios_unity_pod)
* [unity-carthage](https://blog.kakeragames.com/2016/04/30/unity-carthage.html)