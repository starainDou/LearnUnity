> ### 鼠标单击

```
Observable.EveryUpdate()
	.Where(_ => Input.GetMouseButtonUp(0))
	.First()
	.subscribe(_ => { // do something});
```

> ### 按钮单击

```
ButtonA.OnClickAsObservable().Subscribe(_ => { // do something });
```

> ### 按钮双击

```
DoubleClickButton.transform.localPosition = new Vector3(0, 15, 0);
var clickStream = DoubleClickButton.OnClickAsObservable();
clickStream
    .Buffer(clickStream.Throttle(TimeSpan.FromMilliseconds(250)))
    .Where(xs => xs.Count >= 2)
    .Subscribe(xs => Debug.Log("DoubleClick Detected! Count:" + xs.Count)); 
```

> ### 按钮互斥，防止同时点击多个按钮

```
var buttonAStream = ButtonA.OnClickAsObservable().Select(_=>"A");
var buttonBStream = ButtonB.OnClickAsObservable().Select(_=>"B");
var buttonCStream = ButtonC.OnClickAsObservable().Select(_=>"C";

Observable.Merge(
            buttonAStream,
            buttonBStream,
            buttonCStream)
        .First()
        .Subscribe(buttonId => 
        {
            if (buttonId == "A")
            {   
                // Button A Clicked
            }
            else if (buttonId == "B")
            {
                // Button B Clicked
            }
            else if (buttonId == "C")
            {
                // Button C Clicked
            }
        });
```

> ### Toggle点击

```
mToggle.OnValueChangedAsObserble().where(on => on).Subscribe(on => { });
```

> ### Image 拖拽事件

```
mImage.OnBeginDragAsObservable().Subscribe(_ => { });
mImage.OnDragAsObservable().Subscribe(eventArgs => { });
mImage.OnEndDragAsObservable().Subscribe(_ => { });
```

> ### 定时器

```
Observable.Timer(TimeSpan.FromSeconds(5))
	.Subscribe(_ => { /* do something */ })
	.AddTo(this); // 绑定到MonoBehaviour生命周期，this Destroy，定时器一起销毁，避免野指针异常
```

* [实现“点击按钮之后，锁住其他按钮”功能](https://zhuanlan.zhihu.com/p/86750773)
* [UniRx 简介](https://zhuanlan.zhihu.com/p/85663335)
* [UnitRx 响应式编程](https://blog.csdn.net/ChinarCSDN/article/details/84984959)
* [Unity之UniRX](https://blog.csdn.net/qq_14812585/article/details/88596081)