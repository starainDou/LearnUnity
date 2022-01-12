# Camera

> ### Camera简介

要将游戏呈现给玩家，相机是必不可少的。可以对相机进行自定义，脚本化或父子化，从而实现想要的效果。在拼图游戏中，可以让相机处于静止状态，以看到拼图全视图。在第一人称射击游戏中，可以将相机父子化到玩家角色，并将其放置在与角色眼睛等高的位置。在竞速游戏中，您可能希望让相机追随玩家的车辆。   
还可以创建多个相机，并为每个相机指定不同的Depth，相机将按Depth由小到大绘制(大值在上)，还可以设置单个Camera位置和大小，从而实现导弹相机，地图视图，后视镜等微视图。   

> ### Camera属性

* Clear Flags
	
	清除标记，多相机绘制不同元素很方便，默认有4种方式
	**SkyBox**:天空盒(默认)，屏幕空白处显示当前摄像机天空盒，如果没有指定天空盒，则会显示默认背景色
	**Solid Color**:空白处显示此处设置的背景色
	**Depth only**:仅深度，该模式用于对象不被裁剪
	**Don't Clear**:不清除，该模式不清除任何颜色或深度缓存，但这样每帧渲染的画面会叠加到下一帧上(可能花屏)

* Background

	背景，将视线内所有元素都绘制好且无天空盒后应用于剩余屏幕的颜色 
* Culling Mask 

	剔除遮罩层，只渲染选中的layer层
* Projection 
 
	投影模式, 有Perspective透视和Orthographic正交两种

* FOV Axis

	视野轴向，有Vertical垂直视野和Horizontal水平视野两种模式
	
* Field of View

	视野，以度为单位，设置视角宽度，以及相机视口宽高比

* Physical Camera
	
	模拟真实物理相机
	
	**Focal**: 焦距，感光元件与相机镜头之间的距离
	**Sensor Type**:传感器尺寸
	**Sensor Size**:感光元件大小，捕捉图像的传感器的宽高
	**Lens Shift**:镜头位移，从传感器镜头水平和垂直方向进行偏移
	**Gate Fit**:适配模式，决定了Unity如何调整分辨率门的大小(视椎体)

* Clipping Planes

	剪裁平面，相机渲染范围，Near为最近点，Far为最远点
	
* Viewport Rect 

	摄像机在屏幕中的位置和大小
	
* Depth
	
	用于控制多相机时渲染顺序，从小值开始，大值相机在上
	
* Rendering Path

	渲染路径，用什么绘制方法，有五种模式
	**Use Graohics Setting**:用Graphics Setting设置的参数
	**Forward**:正向渲染，所有对象每个材质只渲染一次
	**Deferred**:延迟渲染，所有物体将在无光照的环境中渲染一次，然后在渲染队列尾部将物体的光照一起渲染出来。
	**Legacy Vertex Lit**:旧的顶点照明渲染。光照效果最一般，不支持阴影，一般用于配置较低的移动设备
	**Legacy Deferred**:旧的延迟渲染
	
* Target Texture

	将摄像机画面输出到纹理上
	
* Occlusion Culling

	摄像机是否使用遮挡剔除功能

* HDR

	高动态关照渲染，off为关闭，Use Graphics Setting则使用Graohics Setting设置参数
	
* MSAA

	抗锯齿，off为关闭，Use Graphics Setting则使用Graohics Setting设置参数
	
* Allow Dynamic Resolution

	是否动态分辨率
	
* Target Display

	设置相机画面渲染到指定的显示器
	

> ### Cinemachine 简介

在游戏中，摄像机效果很重要，例如FPS游戏中，需要摄像头跟随角色，还可能做一些第一人称和第三人称的切换，角色进入室内需要调整摄像头位置防止被墙遮挡，使用倍镜需要摄像头观察远处的画面等。如果直接使用Camera，需要写大量控制代码，有了Cinemachine后，一切变得简单起来。   
Cinemachine是Unity 2017推出的一套处理Camera的组件，可以配合Timeline做一些过场动画效果。
[官方文档](https://docs.unity3d.com/Packages/com.unity.cinemachine@2.6/manual/index.html)


* [unity Camera 属性详解](https://blog.csdn.net/qq_34307432/article/details/79277897)
* [Unity2020.1-组件-Camera属性](https://www.pianshen.com/article/81381929455/)
* [Unity Camera组件](https://gameinstitute.qq.com/community/detail/124652)

