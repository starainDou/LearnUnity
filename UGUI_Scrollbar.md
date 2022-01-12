> ### ScrollBar 滚动条简介

ScrollBar 滚动条，是一个用来展示进度比例或位置的组件。
ScrollBar 滚动条组件常与 ScrollView组件配合使用。

> ### ScrollBar 属性

ScrollBar滚动条组件由 Image组件和ScrollBar滑动组件组成

|	属性	|	功能	|
| :---: | :---: |
| Interactable | 是否启动按钮的响应 |
| Transition | 过渡动画类型[none、Color Tint、Sprite Swap、Animation]|
| Target Graphic | 目标图形 |
| Normal Color | 常规状态颜色 |
| Highlighted Color | 高亮状态(鼠标悬停)颜色 |
| Pressed Color | 点击状态颜色 |
| Disabled Color | 禁用状态颜色 |
| Color Multiplier | 颜色乘数 |
| Fade Duration | 渐变持续时间[效果时长] |
| Navigation | 导航类型 |
| Handle Rect | 用来拖动的滑动条对象 |
| Direction | 滚动条方向，可以设置从左到右或从上到下 |
| Value | 滑动条当前所处位置的值 |
| Size | 滑动条的缩放大小 |
| Number Of Steps | 指定可滚动的位置数量，滚动条可滚动的位置数目，为0和1时不生效|
| On Value Changed | 监听函数，值改变时触发 |

Number Of Steps:

步长 = Number Of Steps / 100，如Number Of Steps = 20，那么步长 = 20 / 100 = 1 / 5