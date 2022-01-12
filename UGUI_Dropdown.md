> ### Dropdown 下拉菜单组件简介

Dropdown 下拉菜单，可快速创建大量选择项，无需开发者自己写脚本实现。   

|	层级对象	|	功能	|
| :--- | :---: |
| Label | 显示初始化的文字 |
| Arrow | 显示初始化的下拉箭头 |
| Template | Dropdown的模板样式 |
| Template - Viewport - Content - Item - Item Background | 每一个Item的背景图片 |
| Template - Viewport - Content - Item - Item Checkmark | 每一个Item的下拉框图片 |
| Template - Viewport - Content - Item - Item Label | 每一个Item的文字显示内容 |
| Template - Scrollbar | 下拉框 |

其中Item Background 背景图片和Item Checkmark下拉框图片的图片资源可以修改

> ### Dropdown 下拉菜单属性


|	属性	|	功能	|
| :---: | :---: |
| Interactable | 是否启用组件 |
| Transition | 过渡方式(与Button类似) |
| Navigation | 导航(与Button类似) |
| Template | Drowdown的模板样式，生成的选项都是这个样式 |
| Caption Text | 下拉列表首选项文字显示，可用代码调用获取 |
| Caption Image | 下拉列表首选项的图片显示，可用代码调用获取 |
| Item Text | 每个Item的文字显示 |
| Item Image | 每个Item的图片显示 |
| Value | 会随着Dropdown选择的选项不同而变化 |
| Options | 下拉菜单的选项列表 |
| On Value Changed | 添加监听事件，当Value值变化后，监听事件响应 |

> ### 应用示例

```
private void TestDropdown()
{
    List<String> ipList = new List<string>();
    ipList.Add("1.1.1.1");
    ipList.Add("2.2.2.2");
    ipList.Add("3.3.3.3");
    ipList.Add("4.4.4.4");
    ipList.Add("5.5.5.5");
    
    DropdownIPList = GameObject.Find("DropdownIPList").GetComponent<Dropdown>();
    if (DropdownIPList == null)
    {
        Debug.Log("暂未找到 DropdownIPList");
        return;
    }
    Debug.Log("已经找到 DropdownIPList");
    DropdownIPList.options.Clear(); // 情况默认节点
    DropdownIPList.itemText.fontSize = 13;
    DropdownIPList.captionText.fontSize = 15;
    DropdownIPList.captionText.fontStyle = FontStyle.Bold;
    
    for (int i = 0; i < ipList.Count; i++)
    {
        Dropdown.OptionData option = new Dropdown.OptionData();
        option.text = ipList[i];
        DropdownIPList.options.Add(option);
    }
    // 移除一项
    DropdownIPList.options.Remove(DropdownIPList.options[2]);
    DropdownIPList.options.RemoveAt(0);
    // 不点出下拉选项时默认显示
    DropdownIPList.captionText.text = ipList[3];
    // 添加事件监听
    DropdownIPList.onValueChanged.AddListener(DropdownIPClickAction);
}

private void DropdownIPClickAction(int index)
{
    Debug.Log($"点击的index:{index}");
}
```

> ### 参考

* [【Unity3D-UGUI系列】（七）Dropdown 下拉菜单组件详解](https://blog.csdn.net/q764424567/article/details/119992141)
* [仙魁XAN Unity UGUI 基础 之 DropDown](https://blog.csdn.net/u014361280/article/details/108531150)