> ### 简介

UGUI有很多自动布局组件，如Layout Element、Horizontal Layout Group、Vertical Layout Group、Grid Layout Group、Layout Fitter、Aspect Ratio Fitter

> ### Layout Element

|	属性	|	功能	|
| :---: | :---: |
| Min Width | 最小宽度 |
| Min Height | 最小高度 |
| Preferred Width | 优先预设宽度 |
| Preferred Height | 优先预设高度 |
| Flexible Width | 弹性宽度 |
| Flexibal Height | 弹性高度 |
| Layout Priority | 布局优先 |

分配原则

```
首先分配 Mix With 和 Mid Height
如果还有足够空间，分配 Preferred Width 和 Preferred Height
如果还有额外空间，分配 Flexible Width 和 Flexible Height
```
Flexible 代表占整个的比例，Text、Image Component 会根据内容大小分配Preferred Size

> ### Horizontal Layout Group 水平布局组

|	属性	|	功能	|
| :---: | :---: |
| Padding | 填充内部空间 |
| Spacing | 每个元素间隔 |
| Child Alignment | 当没有填满空间时，子物件对齐位置 |
| Child Controls Size | 按照子控件大小填满空间 |
| Child Force Expand | 强制控制子控件填满空间 |

> ### Vertical Layout Group 垂直布局组

和 Horizontal Layout Group 类似

> ### Grid Layout Group 网格布局组件

|	属性	|	功能	|
| :---: | :---: |
| Padding | 填充内部空间 |
| Cell Size | 每个元素的宽度和高度 |
| Spacing | 每个元素间隔，上下间隔，左右间隔|
| Start Corner | 开始排列的角落位置[左上、右上、左下、右下] |
| Start Axis | 开始排列的轴[水平、垂直] |
| Child Alignment | 当没有填满全部空间时，子物件对齐位置 |
| Constraint | 排列约束限制 |

> ### Content Size Fitter 内容大小装配组件

这个组件控制着父物体的大小，父物体的大小取决于子物体或者设定的比例

|	属性	|	功能	|
| :---: | :---: |
| Horizontal Fit | 水平适应 |
| Vertical Fit | 垂直适应 |

Horizontal Fit和Vertical Fit有三个参数

|	参数	|	介绍	|
| :---: | :---: |
| None | 不调整 |
| Min Size | 根据子物件的Min Width 大小进行调整 |
| Preferred Size | 根据子物件的Preferred 大小进行调整 |

> ### Aspect Ratio Fitter 长宽比装配组件

|	属性	|	功能	|
| :---: | :---: |
| Aspect Mode | 调整模式 |
| None | 不调整 |
| Width Controls Height | 基于Width为基准，依据比例改变Height |
| Height Controls Width | 基于Height为基准，依据比例改变Width |
| Fit In Parent | 依据比例将宽高、位置、锚点自动调整，使此图形在父物件中完全贴齐，此模式可能不会包覆所有空间调整比例 |
| Envelope Parent | 依据比例将宽高、位置、锚点自动调整，使此图形完全包覆父物件，此模式可能会超出空间调整比例 |

Content Size Fitter和 Aspect Ratio Fitter区别：   
Content Size Fitter是通过子物件自动进行调整大小，   
Aspect Ratio Fitter是通过数值[宽高比]进行调整。   


* [【Unity3D-UGUI原理篇】（五）Auto Layout 自动布局](https://blog.csdn.net/q764424567/article/details/120062359)
* [UGUI 使用GridLayoutGroup和Scroll Rect关卡列表](https://www.jianshu.com/p/2ce2f6aa74a1)
* [Unity-UGUI根据标签宽度实现瀑布流布局--FlowLayoutGroup](https://www.jianshu.com/p/746ea26de80b)
* [Unity/Auto Layout -- 理解Layout Elements](https://blog.csdn.net/qq_28849871/article/details/79528697)