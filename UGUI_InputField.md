> ### InputField 简介

InputField是一个用来输入内容的组件，通常用来输入账号、密码、聊天信息等

> ### InputField 属性

|	属性	|	功能	|
| :---: | :---: |
| TextComponent | 用来管理输入的文本组件 |
| Text | 输入的内容 |
| Character Limit | 字符限制类型，可以限制最大字符数的值 |
| Content Type | 内容类型，定义输入内容接受、限制的字符类型 |
| Line Type | 行类型[单行、多行、多行回车换行] |
| Placeholder | 占位符 |
| Caret Blink Rate | 光标闪动频率 |
| Caret Width | 光标宽度 |
| Custom Caret Color | 自定义光标颜色 |
| Selection Color | 选中文本的背景颜色 |
| Hide Mobile Input | 隐藏移动输入内容 |
| Read Only | 是否只读 |
| On Value Changed | 监听事件 |

Content Type

|	选项	|	功能	|
| :---: | :---: |
| Standard | 标准，可以任意输入 |
| Auto corrented | 自动更正[自动更正输入/建议输入内容] |
| Integer Number | 整数 |
| Decimal Number | 允许小数的数字 |
| Alphanumeric | 字母数字[但无法输入符号] |
| Name | 名字模式[支持中文，英文则首字母大写] |
| Email Address | 邮箱地址，允许输入最多一个@符号的字符串 |
| Password | 密码，用星号表示输入的字符，允许任意字符 |
| Pin | 密码，用星号表示输入的字符，仅允许输入整数 |

Line Type

|	选项	|	功能	|
| :---: | :---: |
| Single Line | 单行 |
| Multi Line Submit | 多行 |
| Multi Line Newline | Enter新建多行 |

Input Type

|	选项	|	功能	|
| :---: | :---: |
| Standard | 标准[任意字符] |
| Auto Correct | 自动修正 |
| Password | 密码类型 |

Keyboard Type

|	选项	|	功能	|
| :---: | :---: |
| Default | 平台默认键盘 |
| ASCII Capable | 带标准ASCII键的键盘 |
| Numbers And Punctuation | 键盘与输入|
| URL | URL输入型键盘 |
| Number Pad | 标准数字键盘 |
| Phone Pad | 适合输入电话号码的键盘 |
| NamePhone Pad | 字母数字键盘 |
| Email Address | 适合输入邮件地址的键盘 |
| Nintendo Network Account | 带网络账号键的键盘 |
| Social | 带有社交媒体符号键的键盘 |
| Search | 适合搜索功能的键盘 |

Character Validatior 字符验证类型
 
|	选项	|	功能	|
| :---: | :---: |
| None | 无 |
| Integer | 整型 |
| Decimal | 小数 |
| Alphanumeric | 数字字母 |
| Name | 名字 |
| Email Address | 邮箱地址 |

> ### 代码限制输入字符

限制输入的字符串0-9 a-f A-F

```
using System.Text.RegularExpressions;
using UnityEngine;
using UnityEngine.UI;

public class Input_Test : MonoBehaviour
{
    InputField m_InputField;

    private void Awake()
    {
        m_InputField = GetComponent<InputField>();
        m_InputField.onValueChanged.AddListener(OnInputFieldValueChang);
    }

    private void OnInputFieldValueChang(string inputInfo)
    {
        Regex reg = new Regex("^[A-Fa-f0-9]+$");
        if (reg.IsMatch(inputInfo))
        {
            m_InputField.text = inputInfo;
        }
        else
        {
            if (m_InputField.text == "")
            {
                m_InputField.text = "";
            }
            else
            {
                m_InputField.text = inputInfo.Substring(0, inputInfo.Length - 1);
            }
        }    
    }
}
```
 
> ### 参考文章

* [newImage.GetComponent<Image>().color = Color.gray;](https://blog.csdn.net/qq_49903862/article/details/119826971)
* [正则表达式应用](https://itmonon.blog.csdn.net/article/details/108072717)