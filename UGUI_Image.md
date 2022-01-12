> ### Image 属性

|	属性	|	功能	|
| :---: | :---: |
| Source Image | 来源图片 类型为:Sprite(2D and UI) |
| Color | 颜色 |
| Material | 材质 |
| Raycaste Target | 是否开启射线检测 |
| Image Type | 拖入来源图片后可见，图片类型属性|

> ### Image Type

##### Simple

简单模式，默认   
Use Sprite Mesh 使用图片网格   
Preserve Aspect 锁定比例，等比例缩放 

##### Sliced 

切片模式[九宫格]，需要设置图片边框信息(见SpriteEditor)   
实现图片局部缩放
Fill Center: 是否将图片向中心填充
Pixels Per Unit Mul: 每单位像素数(为0可能有异常)

##### Tiled 

平铺模式，按照图片当前比例向目标区域进行平铺   
Fill Center: 是否将图片向中心填充
Pixels Per Unit Mul: 每单位像素数(为0可能有异常)

##### Filed

填充模式[进度模式]

|	属性	|	功能	|
| :---: | :---: |
| Fill Method | 填充方法 |
| Fill Origin | 填充起点 |
| Fill Amount | 填充量 |
| Clockwise | 是否顺时针方向 |
| Preserve Aspect | 是否保持图片比例 |

> ### 通过代码创建一个Image组件对象

```
// 创建一个Image对象
GameObject newImage = new GameObject("Image");
// 把newImage对象变成Canvas对象的子节点对象
newImage.transform.SetParent(GameObject.Find("Canvas").transform);
// 添加Image组件
newImage.AddComponent<Image>();
// 动态加载贴图赋值给Image
newImage.GetComponent<Image>().sprite = m_FaceSprite.Value;
newImage.transform.localPosition = new Vector3(200, 130, 0);
newImage.GetComponent<Image>().rectTransform.sizeDelta = new Vector2(100, 100);
newImage.GetComponent<Image>().color = Color.gray;
```

> ### Sprite Editor

选中图片，在Inspector窗口，修改Texture Type 为 Sprite(2D and UI)，点击 Sprite Editor 可设置边框

使用 sprite editor 时出现
no sprite editorwindow registered please download 2d sprite

解决方案：Window -> Package Manager -> Unity Registry -> 2D Sprite -> 右下角 intall


<!-- https://blog.csdn.net/q764424567/article/details/119989789 -->