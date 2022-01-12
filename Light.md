> ## Unity的三种光

#### 点光源 Point

点光源，从一个位置向四面八方发出光，就像一盏灯。点光源光线从某一点向各个方向发射。游戏中常用于爆炸、灯泡等。点光源的阴影很耗性能，点光源可以有cookies

#### 平行光 Directional

又称方向光源，光源足够远，足够大。方向光主要模拟阳光或月光。方向光相对最不耗资源。

#### 聚光灯 Spot

> ## 灯光的属性

* Type: 类型，灯光对象的当前类型
* Directional 方向光
* Point: 点光
* Spot: 聚光
* Range: 范围，光从物体的中心发射能到多远，只对点光源和聚光灯。
* Spot Angle: 聚光灯角度，只用于聚光灯
* Color: 光线的颜色
* Intansity: 强度，光线明亮程度，点光源和聚光灯默认1 方向光源默认0.5
* Cookie 这个纹理的alpha通道作为一个遮罩，使光线在不同的地方有不同的亮度。聚光灯和方向光时是一个2D纹理，点光源时必须是一个立方图(Cubemap)
* Cookie Size 缩放Cookie投影，只用于方向光
* Shadow Type 阴影类型
	1. Strength 硬度，阴影的黑暗程度，范围0-1
	2. Resolution 分辨率，阴影的细腻程度
	3. Bias 偏移，灯光空间像素位置与阴影贴图的值比较的偏移量。当值太小，表面产生影子痤疮 self-shadow(物体表面有来自自身的阴影，物体就像长了痘痘一样)，当值太大，光源就会脱离了接收器
	4. Softness 柔化，缩放半阴影区，模糊样本的偏移量，只用于方向光
	5. Softness Fade 柔化淡出，根据相机距离进行淡出，只用于方向光
	
* Draw Halo 绘制光晕，勾选时，光线有一定半径范围的球状光晕被绘制
* Flare 耀斑，可选，在光的位置渲染出来
* Render Mode 渲染模式，灯光重要性，影响照明的保真度和性能
	1. Auto 自动 为桌面构建目标渲染的方法是根据灯光的亮度和当前的质量设置(Quality Setting)在运行时确定
	2. Important 重要，灯光是逐个像素渲染，只用在一些非常重要的效果时（如玩家车头灯）
	3. Not Important 不重要，灯光以最快的速度渲染。定点/对象光模式
* Culling Mask 消隐遮罩，有选择地使组对象不受光的效果影响
* Lightmapping 光照贴图
	1. RealtimeOnly 实时计算
	2. Auto 自动
	3. BakedOnly 仅烘焙bei

> ## 灯光的性能考虑

灯光有两种渲染方式：顶点(Vertex)光照和像素(Pixel)光照。   
顶点光照只计算游戏模型的顶点照明和通过插值计算得出模型表面所有光线。   
像素光照渲染的慢，但它可以实现一些定点光照不能实现的效果。   
正规映射Normal-mapping，灯光Cookies Light cookies，实时阴影 Realtime shadows只能像素光照渲染。   
在像素光照模式下，聚光灯的形状和点光源亮点效果更好。   

灯光对渲染速度有非常大的影响，因此必须权衡照明质量和游戏速度。   
由于像素光照比定点光照奢侈的多，Unity将只在最亮的光逐个像素渲染。   



> ## 参考文章

* [灯光学习](https://blog.csdn.net/q764424567/article/details/78297912)
* [光束 GitHub](https://github.com/kodai100/Unity_LightBeamPerformance)