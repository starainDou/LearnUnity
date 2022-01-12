



| 名称			| Editor 	| Runtime	| 状态	      |
| :-----:  	| :-----:	| :-----:	| :-----:    |
| FairyGUI 	| × 		| √			| 有些公司在用 |
| IMGUI		| √			| √			| 不太了解    |
| NGUI			| ×			| √			| 大部分不用了 |
| UGUI			| ×			| √			| 估计未来被unity抛弃|
| UIElements	| √       | √			| 好像官方力推|
| UIWidgets	| ×       | √			| 中国团队出的，好像凉了|


> ### UGUI简介

GUI [ˈɡuːɪ] 即Graphical User Interface（用户图形界面），在Unity还未更新UGUI之前最为流行的UI插件为NGUI。NGUI可以说是UGUI的前身，Unity团队将NGUI的开发人员收到自己的开发团队，推出UGUI，从Unity4.6开始，UGUI集成到了编辑器中。

UGUI有灵活、快速、可视化的特点。   

UGUI对于开发者来说：

效率高；   
实现效果好；   
易于使用和扩展；   
与Unity编辑器兼容高；


> ### UGUI基本控件

##### Canvas [ˈkænvəs] 画布

每当创建一个UI物体时，Canvas都会自动创建，所有的UI元素都必须是Canvas的子物体。和Canvas一同创建的还有一个EventSystem，是一个基于Input的事件系统，可以对键盘、触摸、鼠标、自定义输入等进行处理。

* Render Mode

	Screen Space - Overlay: 场景顶部渲染，让UI始终位于界面最上面部分      
	Screen Space - Camera: 场景摄像机前，赋值一个相机，按照和相机的距离前后显示物体和UI      
	World Space: 世界坐标，让画布变成一个3D物体可以移动等
	
* UI Scale Mode

	Constant Pixel Size: ['kɑnstənt] ['pɪks(ə)l] 固定像素尺寸，根据像素大小计算UI位置和尺寸。  
	Scale Factor: [skeɪl]['fæktər] UI缩放系数
	Scale With Screen Size: 根据不同屏幕尺寸自动改变UI大小
	Reference Resolution: ['ref(ə)rəns][.rezə'luʃ(ə)n] 参照分辨率，一般设置为1920*1080，UI整体将根据16:9的方式适应缩放   
	Constant Physical Size: 根据固定物理像素
	
* Graphic Raycaster 图形射线
	
	只有UI的元素才继承了Graphic基类，才能响应图形射线。一般保持默认值
	
	Ignore Reversed Graphic: 图形翻转后点击无效
	Blocking OBjects: 阻挡点击物体(当UI前面有物体时，点击前面的物体，射线是否阻挡)
	Blocking Mask: 阻挡层级

> ### Text 

Line Spacing: 行间距   
Rich Text: 富文本(可以结合多种字体类型和大小，寻找文本中的标记标签，和HTML中对字体的类型设置很像)   
Alignment: 对齐方式   
Align By Geometry: 几何方向的对齐   
Horizontal Overflow: 横向溢出的处理方式。文本水平超出最大宽度时，是自动换行还是就溢出不显示。   
Vertical Overflow: 纵向溢出的处理方式。   
Best Fit: 是否忽略对文字大小的设置。选中文字会自动改变大小来全部显示出来。   
Raycast Target: UGUI创建的所有组件都会默认勾选。UI事件会在EventSystem的Update的Progress触发。UGUI会遍历所有Raycast Target是true的UI，发射射线找到玩家最先触发的那个UI，抛出事件给逻辑层去响应   

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

> ### Image

Source Image：转为Sprite格式的图片可以赋值   
Preserve Aspect: 图像宽高是否按原始比例   
Set Native Size: 返回原始大小   

Image Type:

	Simple：在拉伸区域内完全显示一张图片   
	Sliced: 按九宫格显示，九宫格在图片资源中设置，拉伸时九宫格四周大小不变，上下只会左右拉，左右只会上下拉。   
	Tiled: 平铺，在选中范围内显示n张原始大小的图片
	Filled: 按各种方法切割图片(经常用于技能冷却)
	
```
public Image image;
public Sprite sprite;

void Start()
{
	image = transform.GetComponent<Image>();
	image.sprite = sprite;
}
```

> ### RawImage

Image显示的是Sprite格式的图片。   
RawImage显示的是Texture格式的图片。   
RawImage一般用于背景，图标上，用于显示外部图片。   
当显示一张外部加载的图片且不用交互时，RawImage所有时间远低于Image。  

UV Rect: 纹理坐标，可以用其实现帧动画。

RawImage另一个使用呢技巧：在2D界面上实现3D效果。  

1. 新建一个Render Texture，赋值到相机的Target Texture，用于获取相机的3D显示内容。
2. 把Render Texture赋值到RawImage，让RawImage接受相机的3D内容。
3. 再新建个相机，就可以在新建相机2D界面添加3D内容。  

> ### Button

Interactable: 是否可交互   
Navigation: 按钮导航(在EventSystem中，有个当前被选中的按钮，可以通过方向键或代码控制，使被选中的按钮进行更改)   
Visualize: 可以把按键能导航到的路径可视化，高亮黄色箭头显示

Transition: 过渡方式   

 1. 颜色过渡
 	Target Graphic 过渡效果作用目标。   
 	Normal Color 默认颜色  
 	Highlighted Color: 高亮颜色   
 	Pressed Color: 按下颜色   
 	Disabled Color： 禁用颜色   
 	Color Multiplier: 颜色切换系数   
 	Fade Duration: 衰落延迟   
 2. 图片过渡(基本同理)
 3. 动画过渡

 Button添加点击事件(两种方式)
 
 1. 直接在界面OnClick处添加事件
 2. 通过代码给Button添加点击事件
 
 ```
 public Button btn1;
 public Button btn2;
 
 void Start()
 {
 	btn1.onClick.AddListener(NoParameterAction);
 	btn2.onClick.AddListener( ()=>HaveParameterAction("HelloWorld") );
 }
 
 private void NoParameterAction()
 {
 	Debug.Log("NoParameterAction");
 }
 
 private void HaveParameterAction(string str)
 {
 	Debug.Log("HaveParameterAction {0}", str);
 }
 ```
 	
> ### Toggle  ['tɑɡ(ə)l]

一些选项
Is On: 默认是否选中   
Toggle Transition: 切换是否有过渡效果[Fade/None]   
Graphic: 设置开关要起作用的对象，不一定非要是默认的对号   
Group: 设置分组[把多个Toggle放在同一物体下，在这个物体上添加ToggleGroup，可实现多选]   
On Value Change: 当Toggle值改变时所调用函数   

> ### Slider

Slider目录下   
	Background: 未进度的区域显示图片   
	Fill: 已进度的区域显示图片   
	Handle: 可滑动滑块的图片   

面板一些选项   
	Fill Rect: 已进度区域
	Handle Rect: 可滑动滑块区域
	Direction: 可滑动方向
	Min Value: 可滑动最小值
	Max Value: 可滑动最大值
	Whole Number: 是否按整型改变值
	Value: 当前值
	On Value Changed: 值改变时，触发的事件
	
> ### Scrollbar

Size: 可滑动滑块的大小
Number Of Steps: 从最小到最大值一共需要几步。设置0或1时效果和Slider基本一样，都可以自由滑动。设置2，滑块只能在最小或最大，只有这两个位置。设置更大则步进。

* [siki老师：Unity_UGUI_游戏界面(手册)](https://zhuanlan.zhihu.com/p/51027750?from_voters_page=true)   
* [UGUI入门基础](https://www.cnblogs.com/Fflyqaq/p/10598025.html)   
* [UGUI Text组件扩展](https://gameinstitute.qq.com/community/detail/132823)  
* [Unity UGUI 效果 之 ScrollView 轻松实现滚动（内容多少大小可以动态自动调整）预览效果](https://blog.csdn.net/u014361280/article/details/108916319)      
* [Unity手册](https://docs.unity3d.com/cn/2021.1/Manual/UIVisualComponents.html)
* [Unity UI我该学那个](https://blog.csdn.net/qq_35030499/article/details/108738785)
* [Unity UGUI Button扩展](https://www.cnblogs.com/sy-liu/p/12860034.html)
* [UGUI的EventTriggerListener，封装长按、双击、点击、拖拽等多种事件](https://segmentfault.com/a/1190000021824843?utm_source=tag-newest)
* [Unity3D 官方文档 UGUI总览 自动布局组件的介绍](https://blog.csdn.net/u012632851/article/details/77008230)
* [UGUI聊天消息气泡 随文本内容自适应](https://www.cnblogs.com/guxin/p/unity-ugui-chat-item-self-adaption.html)
* [unity性能优化-UGUI](https://www.cnblogs.com/wang-jin-fu/p/8337745.html)
* [Unity中GetComponent的三种方式效率比较](https://www.blinkedu.cn/index.php/2020/12/07/unity中getcomponent的三种方式效率比较/)
* [马三小伙儿 UGUI相关](https://github.com/XINCGer/Unity3DTraining/tree/master/UGUITraining)
* [UGUI优化之RaycastTarget Checker](https://gameinstitute.qq.com/community/detail/132844)
* [UGUI之修改Text字间距](https://blog.csdn.net/qq_26999509/article/details/51902551)
* [Unity UGUI实现圆形Image](https://blog.csdn.net/baidu_39447417/article/details/100179651)
* [[Unity] UGUI远离Mask，实现圆形的Image](https://www.jianshu.com/p/8eaa545418b1)
* [Unity中通过mask组件裁剪出圆形图片，制作出圆形头像](https://www.pianshen.com/article/25681066908/)
* [Unity3D UGUI滚动视图(ScrollView)的使用教程](https://gameinstitute.qq.com/community/detail/122959)
* [UGUI 使用GridLayoutGroup和Scroll Rect关卡列表](https://www.jianshu.com/p/2ce2f6aa74a1)
* [Unity-UGUI根据标签宽度实现瀑布流布局--FlowLayoutGroup](https://www.jianshu.com/p/746ea26de80b)
* [[Unity 3d] 判断手指/鼠标按下触发在UI上的正确方法](https://www.jianshu.com/p/212b3c1490f2)
* [MojoUnity-TextPro：一个简洁高效的UGUI-Text图文混排（带事件处理）的扩展实现](https://scottcgi.blog.csdn.net/article/details/117663440)
* [刘海屏的适配](https://zhuanlan.zhihu.com/p/126699544)


Text首行缩进
使用全角空格
全角空格，可以实现缩进
添加透明字符
比如说，想在首行缩进两个汉字字符，可以在首行添加“[00000000]汉字[-] ”来实现，文本内容首行为“汉字”俩字，但是“汉字”为透明，也达到了缩进的需求

