> ### 属性

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
| OnClick | 点击事件 |

以上是Color Tint 过渡效果下属性功能，Sprite Swap和Animation类似

> ### 绑定事件

* 编辑器可视化绑定事件

点击Inspector的 OnClick 下 + 号，然后把绑定脚本的对象赋值上

* 通过代码

```
public class ButtonTest : MonoBehaviour
{
    public Button m_Button;
    void Start()
    {
        m_Button.onClick.AddListener(ClickEvent);
    }
    public void ClickEvent()
    {
        Debug.Log("鼠标点击");
    }
}
```

* 通过射线监听点击点击的物体来绑定事件[不建议用]

```
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.EventSystems;
using UnityEngine.UI;

public class ButtonTest : MonoBehaviour
{
    void Update()
    {
        if (Input.GetMouseButtonDown(0) && OnePointColliderObject() != null)
        {
            if (OnePointColliderObject().name == "Button" || OnePointColliderObject().name == "Text")
                {
                    ButtonOnClickEvent();
                }
        }
    }

    //点击对象获取到对象的名字
    public GameObject OnePointColliderObject()
    {
        //存有鼠标或者触摸数据的对象
        PointerEventData eventDataCurrentPosition = new PointerEventData(EventSystem.current);
        //当前指针位置
        eventDataCurrentPosition.position = new Vector2(Input.mousePosition.x, Input.mousePosition.y);
        //射线命中之后的反馈数据
        List<RaycastResult> results = new List<RaycastResult>();
        //投射一条光线并返回所有碰撞
        EventSystem.current.RaycastAll(eventDataCurrentPosition, results);
        //返回点击到的物体
        if (results.Count > 0)
            return results[0].gameObject;
        else
            return null;
    }

    public void ButtonOnClickEvent()
    {
        Debug.Log("鼠标点击");
    }
}
```

* 通过EventTrigger 实现按钮点击事件

```
using UnityEngine;
using UnityEngine.EventSystems;
using UnityEngine.UI;

[RequireComponent(typeof(EventTrigger))]
public class ButtonTest : MonoBehaviour
{
    void Start()
    {
        Button btn = transform.GetComponent<Button>();
        EventTrigger trigger = btn.gameObject.GetComponent<EventTrigger>();
        EventTrigger.Entry entry = new EventTrigger.Entry
        {
            // 鼠标点击事件
            eventID = EventTriggerType.PointerClick,
            // 鼠标进入事件 entry.eventID = EventTriggerType.PointerEnter;
            // 鼠标滑出事件 entry.eventID = EventTriggerType.PointerExit;
            callback = new EventTrigger.TriggerEvent()
        };
        entry.callback.AddListener(ClickEvent);
        // entry.callback.AddListener (OnMouseEnter);
        trigger.triggers.Add(entry);
    }

    public void ClickEvent(BaseEventData pointData)
    {
        Debug.Log("鼠标点击");
    }
}
```

* 通过通用类 UIEventListener 来处理Button响应事件

UIEventListener.cs

```
using UnityEngine;
using UnityEngine.EventSystems;

public class UIEventListener : MonoBehaviour, IPointerClickHandler
{
    // 定义事件代理
    public delegate void UIEventProxy();
    // 鼠标点击事件
    public event UIEventProxy OnClick;

    public void OnPointerClick(PointerEventData eventData)
    {
        if (OnClick != null)
            OnClick();
    }
}
```

ButtonTest.cs

```using UnityEngine;
using UnityEngine.EventSystems;
using UnityEngine.UI;

[RequireComponent(typeof(EventTrigger))]
public class ButtonTest : MonoBehaviour
{
    void Start()
    {
        Button btn = this.GetComponent<Button>();
        UIEventListener btnListener = btn.gameObject.AddComponent<UIEventListener>();

        btnListener.OnClick += delegate () {
            ClickEvent();
        };
    }

    public void ClickEvent()
    {
        Debug.Log("鼠标点击");
    }
}
```

> ### 问题解决

点击无结果

1. 如果完全没有点击效果，可能被遮挡
2. 有点击效果，但没反应，可能没获取按钮，没绑定点击事件，绑定函数错误等

<!-- https://blog.csdn.net/q764424567/article/details/119989323 -->