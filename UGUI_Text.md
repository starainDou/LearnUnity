> ### 属性

|	属性	|	说明	|
| :---: | :---: |
| Text | 用于显示的文本 |
| Font | 字体 |
| Font Style | 字体样式(正常、加粗、斜体等) |
| Font Size | 字号 |
| Line Spacing | 行间距 |
| Rich Text | 富文本 |
| Alignment | 对齐方式[横线、纵向] |
| Align By Geometry | 几何对齐，是否以显示的最大长宽而不是字体长宽对齐|
| Horizontal Overflow | 文本横向溢出方式[Wrap换行、Overflow溢出] |
| vertical Overflow | 文本纵向溢出方式[Truncate截断、Overflow溢出]|
| Best Fit | 忽略FontSize，自适应改变文字大小 |
| Color | 文本颜色 |
| Material | 通过材质渲染文本以拥有炫酷效果 |
| Raycast Target | 射线检测，通常关闭，因为文本最好只用来显示 |

示例


```
private Text text;

void Start() 
{
	text = transform.GetComponent<Text>();
	text.text = "Hello";
	text.fontSize = 20;
	text.color = Color.blue;
	text.fontStyle = FontStyle.Bold;
}
```

> ### 问题解决

#### 文本颜色

可以编辑器Inspector中修改颜色，还可以代码中设置

```
text.color = new Color(1f, 10 / 255f, 0, 0.9f)
```

也可以富文本用标签设置文本颜色

```
<color=red> New Text </color>
```

#### 文本不显示

1. 遮挡
2. 颜色和背景相似或相同
3. 字体超过文本框
4. 屏幕分辨率造成Scene和Game窗口显示不同[通过修改Canvas Scaler解决]
5. 不显示内容，但显示文本组件，可能字体丢失

> ### 附加文章

* [ugui设置字体间距（支持多行）](blog.sina.com.cn/s/blog_bcbac6b90102x05i.html)
* [ugui-text渐变（支持多行）](blog.sina.com.cn/s/blog_bcbac6b90102x01a.html)
* [Unity3D-UGUI Text 文本调整字间距](https://blog.csdn.net/Memoryuuu/article/details/85627091)
* [UGUI Text组件扩展](https://gameinstitute.qq.com/community/detail/132823)