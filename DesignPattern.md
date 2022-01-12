> ### 23种设计模式(Design Pattern)

#### 

> ### 七大设计原则(Design Principle)

#### 单一职责原则

用于控制类的颗粒度大小。一个对象应该只包含单一的职责，并且该职责被完整的封装在一个类中，就一个类而言，应该仅有一个引起它变化的原因。一个类(大到模块，小到方法)承担的职责越多，他被复用的可能性越小。当一个职责变化时，可能会影响到其他职责的运作。将不同的职责分离封装到不同类中，将不同的变化原因封装到不同的类中。单一职责是实现高内聚、低耦合的知道方针。

#### 开闭原则 (开放封闭原则)

软件实体应当对于扩展开放，对修改关闭，软件实体应尽量在不修改原有代码的情况下进行扩展。在开闭原则的定义中，软件实体可以是一个软件模块、一个由多个类组成的局部结构或一个独立的类。抽象化是开闭原则的关键(相对稳定的抽象层+灵活的具体层)

#### 里氏替换原则 (向量转型原则，代码抽象)

所有引用基类的地方必须能透明地使用其子类的对象。在软件中将一个基类对象替换成子类对象，程序不会产生任何错误或异常，反过来不一定成立(软件实体使用子类对象时，换成基类对象不一定能够正常使用)。在程序中尽量使用基类类型来对对象进行定义，而在运行时再确定其子类类型。

#### 依赖倒置原则

高层模块不应该依赖底层模块，他们都应该依赖抽象。抽象不应该依赖细节，细节应该依赖于抽象。要针对抽象编程，而不应针对实现编程。在程序代码中传递参数时或在关联关系中，尽量引用层次高的抽象层，即用接口和抽象类进行变量类型声明、参数类型声明、方法返回类型声明、以及数据类型的转换等。在程序中尽量使用抽象层进行编程，而将具体类写在配置文件中。针对抽象层编程，将具体类的对象通过依赖注入(Dependency Injection)方式注入到其他对象。

* 构造注入(使用构造器注入，附加到对象上)
* 设值注入(Setter注入)
* 接口注入

#### 接口隔离原则(约束，抽象，标签[仅限于Unity])

客户端不应该使用依赖那些它不需要的接口。当一个接口太大时，需要将它分割成一些更细小的接口。使用该接口的客户端仅需知道与之相关的方法即可。每一个接口应该承担起一种相对独立的角色，不干不该干的事，该干的事都要干。

#### 合成复用原则

优先


> ### 参考

* [Unity_设计模式_设计原则_02](https://blog.csdn.net/qq_39710961/article/details/78062798)
* [C#之常用设计模式(unity版本)](https://blog.csdn.net/weixin_39923777/article/details/83142410)
* [设计模式从零到一之六大原则](https://mp.weixin.qq.com/s/yn8OhRPZZ-1D_FuK4yzj0Q)
* [各种设计模式的Unity3D C#版本实现](https://github.com/QianMo/Unity-Design-Pattern)