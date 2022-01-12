> ### ScrollView 滚动视图简介

ScrollView滚动视图组件是一个滚动显示内容的组件，比如显示背包、商城等有大量物品时，可用ScrollView滚动显示。

> ### ScrollView 滚动视图层次

|	层次	|	说明	|
| :---: | :---: |
| Viewport | 显示的窗口 |
| Content | 显示的内容 |
| Scrollbar Horizontal | 水平滚动条 |
| Scrollbar Vertical | 垂直滚动条 |

> ### ScrollView 滚动视图属性

|	属性	|	说明	|
| :---: | :---: |
| Content | 滚动的内容区域，所有子物体都会显示在滚动内容区|
| Horizontal | 是否启动水平滚动 |
| Vertical | 是否启动垂直滚动 |
| Movement Type | 滑动框弹性模式[Unrestricted不受限、Elastic弹性、Clamped夹紧] |
| Elasticity | Elastic弹性模式中的反弹量[其他模式无]|
| Intertia | 惯性，拖动结束会根据惯性继续移动，未设置仅在拖动时移动 |
| Decaleration Rate | 减速度，0立即停止，1永不停止 |
| Scroll Sensitivity | 灵敏度[滚轮灵敏程度] |
| Viewport | 视图接口，是Content的父物体 |
| Horizontal Scrollbar | 底部水平滚动条 |
| Visibility | 可见度，如果显示的内容没有超出Viewport自动隐藏，也可以选择展开视口 |
| Spacing | 选择自动隐藏并展开视口时，滚动条和视口之间的距离 |
| Vertical Scrollbar | 右部竖直滚动条 |

> ### Movement Type 弹性模式介绍

* Unrestricted 
不受限模式，拖动结束减速到0就待在该位置，由于拖动范围不限制，可能会拖出视口外。  

* Elastic 
弹性模式，拖动结束会根据弹性变量进行回弹，拖动范围是受限的。

* Claped 
夹紧模式，带范围控制，视口内和别的模式一样，临界时就不能再拖动了。

> ### Visibility 能见度

|	模式	|	说明	|
| :---: | :---: |
| permanent | 永久显示 |
| Auto Hide | 自动隐藏[Content不超出Viewport范围才会隐藏] |
| Auto Hide And Expand Viewport | 自动隐藏并扩展视口 |




* [无限循环滚动 UGUICircularScrollView](https://github.com/rlafydid/UGUICircularScrollView)
* [Unity3D UGUI滚动视图(ScrollView)的使用教程](https://gameinstitute.qq.com/community/detail/122959)
* [Unity 使用UGUI创建可重用TableView](https://blog.csdn.net/tmac3380809/article/details/51290387)
* [UGUI-TableView](https://github.com/GeWenL/UGUI-TableView)
* 