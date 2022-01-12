TextMeshPro是默认文本组件的增强版，拥有和Text一样的高性能，他使用了Signed Distance Field (SDF) 技术，可以轻松更改效果，且矢量化(同样需要将字符放到一个图集，但不使用像素代表字符形状，而是使用SDF技术来生成字符形状)，放大无毛边。每个TextMeshPro都附带可以调整的材质，从而修改文本风格，TextMeshPro拥有Text组件所有的变量且更多其他变量。
第一次使用TextMeshPro会提示添加响应资源，都点击添加后即可。

> ### 有两个TextMesh Pro组件

*  TextMesh Pro文本对象 GameObject -> 3D Object -> TextMesh Pro
*  TextMesh Pro文本组件 GameObject -> UI -> TextMesh Pro

> ### TextMesh Pro组件两部分组成

TextMesh Pro组件分两部分：Text Input Box(文本输入) 和 Font Setting(字体设置)

#### Text Input Box 文本输入

```
<b><i><u>Bold Italics UnderLine</u></i></b>
<s>Strikethrough</s> <sup>Superscript</sup> <sub>Subscript</sub> <size=13>13</size> <size=+4>17</size><size=-2>11</size>
<pos=20>20Margin<color=yellow>Yellow</color> \nNew Text
```

可以输入多行文本，键盘上按Enter会换行，输入\n会换行，支持富文本

#### Font Setting 字体设置

|	属性	|	功能	|
| :---: | :---: |
| Font Asset | 字体资源 |
| Material Preset| 预设材质 |
| Font Style | 全局样式[粗体、斜体等] |
| Font Size | 全局字号 |
| Auto Size | 动态调整大小[在最大值和最小值之间] |
| Vertex Color | 顶点颜色，全局字符颜色 |
| Color Gradient | 颜色渐变 |
| Override Tags |  启用覆盖颜色标签 |
| Spacing Options | Character字间距、Word词间距、Line行间距、Paragraphic段落间距 |
| Alignment | 对齐方式 |
| Wrapping | 是否支持换行 |
| Overflow | 溢出模式 |
| Horizontal Mapping | 水平映射方式 |
| Vertical Mapping | 垂直映射方式 |

#### Extra Setting 其他设置 

Material Editor Feature Panels 材质编辑器功能面板

Face Panel

|	属性	|	功能	|
| :---: | :---: |
| color | 选择字符表面的颜色和透明度(该颜色将与定点颜色相结合) |
| Softness | 控制字体的柔软度 |
| Dilate | 膨胀[增大或减小字符大小] |
| Texture | 字符表面纹理(Color会影响纹理颜色，纹理选项不是在所有着色器上都能用) |
| Gloss | 控制字符表面亮度 |

Outline Panel

|	属性	|	功能	|
| :---: | :---: |
| color | 控制字符轮廓的颜色和透明度 |
| Thickness | 控制轮廓的厚度 |

Underlay Panel 下划线可用于向文本对象添加阴影或边框

|	属性	|	功能	|
| :---: | :---: |
| Color | 控制衬底的颜色和透明度 |
| Offset(X, Y) | 控制衬底的位置 |
| Dilate | 增加或减少衬底的尺寸 |
| Softness | 控制衬底的柔软度 |

Bevel Panel

内外斜面两种类型模拟三维斜面在二维物体上的视觉外观

|	属性	|	功能	|
| :---: | :---: |
| Amount | 控制斜面的应用数量 |
| Offset | 控制斜面相对于字符表面边缘的位置 |
| Width | 控制斜面效果的宽度 |
| Roundness | 确定斜面脊线是尖锐的还是圆形的 |
| Clamp | 限制斜面的高度 |

Lighting Panel 

灯光设置控制斜面，凹凸和外观的环境映射




> ### 中文支持问题

TextMesh Pro 字符集默认不支持中文，通过配置Asset以支持中文。

Window -> TextMeshPro -> Font Asset Creator  
弹出Font Asset Creator 弹窗

完成后Generate Font Atlas再Save得到.asset文件

> ### 参考文章

* [Unity3d TextMeshPro教程](https://blog.csdn.net/elineSea/article/details/88799896)
* https://www.pianshen.com/article/16891996402/
* [TextMesh Pro常用标签](https://www.shuzhiduo.com/A/lk5aQWjm51/)
* [Unity textMeshpro 显示中文设置](https://blog.csdn.net/jk_chen_acmer/article/details/106816562)
* [Unity TextMeshPro替代Text组件创建简体中文字体纹理集](https://www.cnblogs.com/koshio0219/p/11643268.html)
* [在Unity中使用TextMeshPro组件](https://www.ifeelgame.net/tools/在unity中使用textmeshpro组件/)