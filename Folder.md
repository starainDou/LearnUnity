Editor: 存放编辑器脚本，不会被打包，并且可以在任意位置添加多个Editor文件夹，里面的脚本不允许挂载到游戏对象上

Plugins: 存放插件、.so、.jar、.a、三方库等，会被打包，且这个文件夹只能是Assets文件夹的直接子目录

Editor Default Resources：存放编辑器需要的资源，中间空格不能省，使用EditorGUIUtility.Load加载，文件夹只能有一个，只能是Assets文件夹的直接子目录。

Gizmos：存放在Scene视图中显示的图片，只在scene显示，发布游戏以后就不会运行了。使用Gizmos.DrawIcon加载图片，继承MonoBehaviour每帧都会执行OnDrawGizmos

```
void OnDrawGizmos() 
{
    Gizmos.DrawIcon(transform.position, "123.png", true);
}
```

Resources：存放游戏运行过程中加载的资源，一个只读的文件夹，资源会被压缩，加密。不管有没有使用都会被打包进.apk或者.ipa，使用Resources.Load加载，可以在任意位置添加多个，最好存放预制体，预制体引用的贴图可以再到Resources外，免得把没有使用的资源打到安装包里面。

StreamingAssets：存放不需要处理的原始文件，一个只读的文件夹，打包时候资源不压缩不加密。所以此文件夹一定要注意大小，打包前多大，就会多大打到包中