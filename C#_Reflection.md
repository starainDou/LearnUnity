> ### 反射基础知识

反射指程序可以访问、检测或修改他本身状态或行为的能力。   
程序集包含模块，而模块包含类型，类型又包含成员。反射则提供了封装程序集、模块和类型的对象。   
可以使用反射动态地创建类型的实例，将类型绑定到现有对象，或从现有对象取类型。还可以调用类型的方法或访问其字段和属性

> ### 反射优缺点

* 优点

 1. 提高了程序的灵活性和扩展性
 2. 降低了耦合性，提高了自适应能力
 3. 它允许程序创建和控制任何类的对象，无需提前硬编码目标类
 
* 缺点

 1. 使用反射基本上是一种解释操作，用于字段和方法接入时要远慢于直接代码。因此反射机制主要应用在对灵活性和拓展性要求很高的系统框架上，普通程序不建议太多使用
 2. 使用反射会模糊程序内部逻辑，开发人员希望在源代码中看到程序的逻辑，反射却绕过了源代码的技术，因此带来维护上的困难

> ### 反射用途

* 它允许在运行时查看Attribute特性信息
* 它允许审查集合中的各种类型，以及实例化这些类型
* 它允许延迟绑定的方法和属性
* 它允许在运行时创建新类型，然后使用这些类型执行一些任务

> ### 示例

```
FieldInfo fieldInfo1 = reason.GetType().GetField(reason.ToString());
            var attributes = fieldInfo1.GetCustomAttribute(typeof(DescriptionAttribute), false);
            
            
            Type type = reason.GetType();
            string name = Enum.GetName(type, reason);
            if (name is null) return "null";

            FieldInfo fieldInfo = type.GetField(name);
            var attribute = Attribute.GetCustomAttribute(fieldInfo, typeof(DescriptionAttribute)) as DescriptionAttribute;
            return (attribute == null || attribute.Description == null) ? name : attribute.Description;
```

https://www.cnblogs.com/chenwolong/p/ENUM.html
* [C#反射（Reflection）详解](https://www.jianshu.com/p/442f52a10b08)