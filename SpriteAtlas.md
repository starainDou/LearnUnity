> ### Sprite Atlas 

不使用图集时，每个纹理发出一次 draw call，当很多纹理时就需要很多次draw call，占用大量资源，对项目性能产生负面影响。
使用精灵图集后，多个纹理合并为一个组合纹理的资源，从而一次draw call 完成原来多次draw call才能完成的任务，以较小的性能开销一次性访问压缩的纹理

> ### 下载 2D Sprite 插件

window -> Package Manager -> Unity registry -> 搜索安装 2D Sprite

> ### Sprite Atlas 设置


Sprite Atlas 将纹理打包成一个纹理图集，进入运行模式时或者构建播放器或AssetBundle时，他会打包这些纹理。

要配置打包行为，需要开启Sprite Packer

Edit -> Project Setting -> Editor -> Sprite Packer -> Model -> Sprite Atlas V2(Experimental)-Enable

* Diabled 
 在项目中禁用精灵图集打包，当项目进入播放模式或发布构建时，不会构建精灵图集。Pack Preview也禁用
 
* Enable for Builds(Legacy Sprite Packer)

选择此模式将启用旧版Sprite Packer并禁用精灵图集(因为两者无法同时启用)，仅针对发布的构建，Unity使用旧版Sprite Packer将精灵进行打包。Editor和播放模式引用原始源纹理而非打包图集中的纹理

* Always Enable(Legacy Sprite Packer)

选择此模式将启用旧版Sprite Packer并禁用精灵图集(因为两者无法同时启用)，Unity使用旧版Sprite Packer将所选纹理打包到图集中，且精灵在运行时将引用打包的纹理。但是，精灵将在编辑模式期间引用原始未打包的纹理
 
* Sprite Atlas V1 - Enable For Builds 

AssetDatabaseV1，打包时启用

* Sprite Atlas V1 - Always Enable

采用AssetDatabaseV1，一直启用

v1的不支持缓存服务器，Unity只能缓存储存在Library文件夹中的Artifact

* Sprite Atlas V2(Experimental) - Enable

仅支持将精灵和纹理拖到 Objects for Packing，不能将文件夹拖到 Objects for Packing

> ### 添加Sprite Atlas

Create -> 2D -> Sprite Atlas

> ### 加载

* Resources加载

```
var atlas = Resources.Load<SpriteAtlas>("图集名称");
var sprite = atlas.GetSprite("精灵路径");
```

* AssetBundle加载

```
var bundle = AssetBundle.LoadFromFile("bundle路径");
var atlas = bundle.LoadAsset<SpriteAtlas>("图集名称");
var sprite = atlas.GetSprite("精灵名称");
```

例

```
var atlas = Resources.Load<SpriteAtlas>("ArrowAtlas");
if (atlas == null)
{
    Debug.Log("atlas is null");
    return;
}

var sprite = atlas.GetSprite("JianTou_1");
if (sprite == null)
{
    Debug.Log("sprite is null");
    return;
}
_Image.sprite = sprite;
```

> ### 属性注释

Type: Master主精灵，Variant变体精灵。默认Master，设置为Variant时会显示额外属性设置。不同分辨率可以采用不同图集，可通过Variant对图集进行缩放控制。变体图集需要在其Master Atlas图集属性中设置主类型的精灵图集。变体图集接收主图集内容的副本用作自己的内容。  

Master Atlas: 变体图集接受其主图集内容的副本以用作自己的内容

Scale: 变体精灵图集的缩放因子，范围[0.1, 1]。变体图集纹理的大小是主图集纹理乘Scale的结果。

Include in Build: 当前构建中包含该精灵图集资源，默认启用

Allow Rotation 允许旋转，最好不勾选

Tight Packing 高密度填充，最好不勾选，根据精灵轮廓而非矩形轮廓可能出现镂空被填充

Padding: 定义精灵图集各个精灵纹理间像素空隙，这是一个缓冲区，防止出现相邻精灵纹理出现重叠

Read/Write Enabled: 允许从脚本函数(例如Texture2D.SetPixels)访问纹理数据。启用此属性，Unity将创建纹理数据的副本，这会使纹理所需内存翻倍，并可能产生性能负面影响。此属性仅适用于未压缩纹理或DXT压缩纹理，因为Unity无法读取其他类型的压缩纹理。

Generate Mip Maps: 生成纹理LOD的Mipmap

sRGB: 将纹理存储在伽马空间

Filter Mode: 选择Unity在变换过程中拉伸打包的纹理如何过滤。此设置将覆盖精灵图集中任何已打包的精灵的Filter Mode设置。





* [Unity 手册文档](https://docs.unity3d.com/cn/2021.1/Manual/class-SpriteAtlas.html)
* [[Unity优化] Unity图集的理解和使用](https://www.jianshu.com/p/bdc39395f768)
* [Unity3d Ugui 23图集Sprite Atlas](https://blog.csdn.net/HexianWHH/article/details/119520969)
* [SpriteAtlas 使用小结](https://blog.csdn.net/jun50713852/article/details/80902555)
* [[Unity优化] Unity图集的理解和使用](https://www.jianshu.com/p/bdc39395f768)
* [unity图集之：从sprite packer到sprite atlas](https://blog.csdn.net/allenwithno3/article/details/79747844)