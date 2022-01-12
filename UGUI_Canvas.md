> ### Canvas 画布

Canvas画布，所有UI元素的父物体，当创建一个UI元素的时候，如果没有Canvas画布就会自动创建一个。一个场景可以有多个Canvas画布组件。
手动创建 Hierarchy -> Create -> UI -> Canvas

> ### Canvas 画布属性

Canvas创建时自带三个组件： Canvas、Canvas Scaler、Graphic RayCaster

#### 1. Canvas

######  Render Mode

* Screen Space - Overlay

屏幕空间覆盖模式，不管有没有相机去渲染场景，让UI始终位于界面最上层

|	属性	|	功能	|
| :---: | :---: |
| Pixel Perfect | UI元素像素对应，效果就是边缘清晰不模糊 |
| Sort Order | 多Canvas时数值大后渲染，值大遮挡值小 |
| Target Display | 目标显示器，若有多个屏幕可选择 |
| Additional Shader Channels | 附加着色通道，决定Shader可读取哪些数据，如法线、切线等 |



* Screen Space - Camera

相机模式，适用于场景模型太多或太大时，按与相机远近渲染，让UI与渲染的相机较远时可避免遮挡，并且Canvas和相机间有一定距离，可以放置模型或粒子特效。

|	属性	|	功能	|
| :---: | :---: |
| Pixel Perfect | UI元素像素对应，效果就是边缘清晰不模糊 |
| Render Camera | 渲染的相机 |
| Plane Distance | Canvas与渲染相机之间的距离 |
| Sorting Layer | 画布深度，指定了相机的渲染顺序 |
| Order In Layer | 值越大，UI显示越靠前 |

* World Space

世界坐标模式，让画布变为一个3D物体一样可以移动，旋转和缩放等。UI元素根据在3D场景中的位置来决定渲染在其他物体的前后

|	属性	|	功能	|
| :---: | :---: |
| Event Camera | 响应事件的相机 |
| Sorting Layer | 画布的深度，指定了相机的渲染顺序 |
| Order In Layer | 值越大，UI显示越靠前 |
| Additional Shader Channels | 附加着色通道，决定Shader可读取哪些数据，如法线、切线等 |

#### 2. Canvas Scaler

###### UI Scale Mode

* Constant Pixel Size

固定像素尺寸，根据像素大小计算UI位置和尺寸。

|	属性	|	功能	|
| :---: | :---: |
| Scale Factor | UI缩放因子 |
| Reference Pixels Per Uit | 单位面积参考像素数量(为0可能显示不出) | 

*  Scale With Screen Size

根据屏幕分辨率和设置的宽高比来调整UI的位置和大小，通常做屏幕自适应时用此模式。一般设置为1920*1080，UI整体将根据16:9的方式适应缩放

|	属性	|	功能	|
| :---: | :---: |
| Reference Resolution | 参考分辨率 |
| Screen Match Mode | 缩放模式 | 
| Match | 宽高比 |

* Constant Physical Size 

固定物理像素尺寸

|	属性	|	功能	|
| :---: | :---: |
| Physical Unit |  使用物理单位 |
| Fallback Screen DPI | 备用屏幕的DPI  |
| Default Sprite DPI | 默认图片的DPI |
| Reference Pixels Per Uit | 单位面积像素数量(为0可能显示不出) |

#### 3. Graphic Raycaster

控制是否让UI响应射线点击，只有UI元素才继承了Graphic基类，才能响应图形射线

|	属性	|	功能	|
| :---: | :---: |
| Ignore Reversed Graphic | 忽略翻转的UI，图形翻转后点击无效 |
| Blocking Objects | 阻挡点击物体，当UI前有物体时，点击前面的物体，射线会被阻挡 |
| Blocking Mask | 阻挡层级，当UI前有设置的层级时，点击前面的物体射线会被阻挡 |

> ### EventSystem

和Canvas同时创建的还有一个EventSystem，是一个基于Input的事件系统，可以对键盘、触摸、鼠标、自定义输入等进行处理。EventSystem负责处理输入、射线投射以及发送事件。一个场景中只能有一个EventSystem组件

#### 1. Transform

|	属性	|	功能	|
| :---: | :---: |
| Position | 位置 |
| Rotation | 旋转 |
| Scale | 缩放 |

#### 2. Event System(Script)

消息机制的核心

|	属性	|	功能	|
| :---: | :---: |
| First Selected | 首选对象 |
| Send Navigation Events | 发送导航事件 |
| Drag Threshold | 拖动阔值 |

#### 3. Standalone Input Module(Script)

处理输入的鼠标事件、触摸事件、进行事件分发，是负责产生输入的组件

|	属性	|	功能	|
| :---: | :---: |
| Horizontal Axis | 横轴 |
| Vertical Axis | 纵轴 |
| Submit Button | 提交按钮 |
| Cancel Button | 取消按钮 |
| Input Actions Per Seconds | 每秒输入动作 |
| Repeat Delay | 重复延迟 |
| Force Module Action | 强制模块激活 |

> ### RectTransform 

RectTransfrom定义了UI元素的位置、旋转、缩放、大小、锚点、轴点等。

> ### EventSystem 运行流程分析

EventSystem就像是为UGUI设计好的消息中心，管理着所有参与消息处理的UGUI组件，比如Panel、Image、Button等。   
EventSystem是消息机制的核心。   
Standalone Input Module是负责产生输入的组件，继承自BaseInputModule实现类，Unity还有其他实现类，用户也可以自定义一个实现类用于事件处理。   
EventSystem响应的是用户的点击、触摸、拖动、长按等，那么怎么确定这些操作针对哪个UI，那就靠GraphicRayCaster组件了。

响应接口

|	接口	|	说明	|
| :---: | :---: |
| IPointerEnterHandler - OnPointerEnter | 当指针进入对象时调用 |
| IPointerExitHandler - OnPointerExit | 当指针退出对象时调用 |
| IPointerDownHandler - OnPointerDown | 当指针压在对象上时调用 |
| IPointerUpHandler - OnPointerUp | 当指针被抬起释放时在原始按下的对象上调用 |
| IPointerClickHandler - OnPointerClick | 当在同一对象上按下和释放指针时调用 |
| IInitalizePotentialDragHandler - OnInitalizePotentialDrag| 在找到拖动目标时调用，用于初始化值 |
| IBeginDragHandler - OnBeginDrag | 当拖动即将开始时在拖动对象上调用 |
| IDragHandler - OnDrag | 当发生拖拽时在拖动对象上调用 |
| IEndDragHandler - OnEndDrag | 当拖动完成时在拖动对象上调用 |
| IDropHandler - OnDrop | 在拖动完成时对对象调用 |
| IScrollHandler - OnScroll | 当滚轮滚动时调用 |
| IUpdateSelectedHandler - OnUpdateSelected | 在选定对象上调用 |
| ISelectHandler - OnSelect | 当对象变成选定对象时调用 |
| IDeselectHandler - OnDeselect | 当被选定的对象被取消选定时调用 |
| IMoveHandler - OnMove | 当移动事件发生时调用(上下左右等) |
| ISubmitHandler - OnSubmit | 按下提交按钮时调用 |
| ICancelHandler - OnCancel | 按下取消按钮时调用 |

代码示例

```
public class EventTest : MonoBehaviour, IPointerClickHandler, IDragHandler, IPointerDownHandler, IPointerUpHandler {

    public void OnDrag(PointerEventData eventData) {
        //当发生拖动时在拖动对象上调用
    }

    public void OnPointerClick(PointerEventData eventData) {
        //当在同一对象上按下和释放指针时调用
    }

    public void OnPointerDown(PointerEventData eventData) {
        //当指针压在对象上时调用
    }

    public void OnPointerUp(PointerEventData eventData) {
        //当指针被释放时调用(在原始按下的对象上调用)
    }
}
```



<!-- https://blog.csdn.net/q764424567/article/details/119923544 -->
[【Unity3D-UGUI原理篇】（三）RectTransform 组件详解](https://blog.csdn.net/q764424567/article/details/120053490)
[【Unity3D-UGUI原理篇】（四）Event System Manager 事件与触发](https://blog.csdn.net/q764424567/article/details/120060856)