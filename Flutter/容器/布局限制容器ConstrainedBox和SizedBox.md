## 介绍
> ConstrainedBox和SizedBox都是通过RenderConstrainedBox来渲染的。SizedBox只是ConstrainedBox一个定制，本节把他们放在一起讨论。

## ConstrainedBox
ConstrainedBox用于对齐子Widget添加额外约束。例如，如果想让子Widget的最小高度为80像素，可以使用`const BoxConstraints(minHeight: 80.0)`作为widget的约束。

我们先定义一个redBox，设置他的背景为红色，不指定高宽
```dart
Widget redBox = DecorateBox(
    decoration: BoxCoration(color: Colors.red),
);
```

我们实现一个最小高度为50，宽度尽可能大的红色容器
```dart
ConstrainedBox(
    constraints: BoxConstraints(
    minWidth: double.infinity,// 尽可能大
    minHeight: 50.0,// 最小高度 50像素
    ),
    child: Container(
    height: 5.0,
    child: redBox,
    ),
);
```
可以看到，我们虽然将Container的高度设置为5像素，但是最终却是50像素，这正是ConstrainedBox的最小高度限制生效了。如果将Container的高度设置为80像素，那么最终红色区域的高度也会是80像素，因为在此示例中，ConstrainedBox只限制了最小高度，并未限制最大高度。

## BoxConstraints
BoxConstraints用于设置限制条件
```dart
const BoxConstraints({
    this.minWidth = 0.0,// 最小宽度
    this.maxWidth = double.infinity,// 最大宽度
    this.minHeight = 0.0,// 最小高度
    this.maxHeight = double.infinity // 最大高度    
})
```
BoxConstraints还定义了一些便捷的构造函数，用于快速生成特定限制规则的BoxConstraints，如`BoxConstraints.tight(Size size)`，它可以生成给定大小的限制；const `BoxConstraints.expand()`可以生成一个尽可能大的用以填充另一个容器的BoxConstraints。

## SizedBox
> 为子Widget指定指定高宽
```dart
SizedBox(
  width: 80.0,
  height: 80.0,
  child: redBox
)
```
实际上SizedBox和只是ConstrainedBox一个定制，上面代码等价于：
```dart
ConstrainedBox(
  constraints: BoxConstraints.tightFor(width: 80.0,height: 80.0),
  child: redBox, 
)
```
而BoxConstraints.tightFor(width: 80.0,height: 80.0)等价于：
```dart
BoxConstraints(minHeight: 80.0,maxHeight: 80.0,minWidth: 80.0,maxWidth: 80.0)
```
而实际上ConstrainedBox和SizedBox都是通过RenderConstrainedBox来渲染的，我们可以看到ConstrainedBox和SizedBox的createRenderObject()方法都返回的是一个RenderConstrainedBox对象：
```dart
@override
RenderConstrainedBox createRenderObject(BuildContext context) {
  return new RenderConstrainedBox(
    additionalConstraints: ...,
  );
}
```

## 多重限制
如果某一个widget有多个父ConstrainedBox限制，那么最终会是哪个生效？我们看一个例子：
```dart
ConstrainedBox(
    constraints: BoxConstraints(minWidth: 60.0, minHeight: 60.0), //父
    child: ConstrainedBox(
      constraints: BoxConstraints(minWidth: 90.0, minHeight: 20.0),//子
      child: redBox,
    )
)
```
上面我们有父子两个ConstrainedBox，他们的限制条件不同。

最终显示效果是宽90，高60，也就是说是子ConstrainedBox的minWidth生效，而minHeight是父ConstrainedBox生效。单凭这个例子，我们还总结不出什么规律，我们将上例中父子限制条件换一下：
```dart
ConstrainedBox(
    constraints: BoxConstraints(minWidth: 90.0, minHeight: 20.0),
    child: ConstrainedBox(
      constraints: BoxConstraints(minWidth: 60.0, minHeight: 60.0),
      child: redBox,
    )
)
```

最终的显示效果仍然是90，高60，效果相同，但意义不同，因为此时minWidth生效的是父ConstrainedBox，而minHeight是子ConstrainedBox生效。

通过上面示例，我们发现有多重限制时，对于minWidth和minHeight来说，是取父子中相应数值较大的。实际上，只有这样才能保证父限制与子限制不冲突。

## UnconstrainedBox
UnconstrainedBox不会对子Widget产生任何限制，它允许其子Widget按照其本身大小绘制。一般情况下，我们会很少直接使用此widget，但在"去除"多重限制的时候也许会有帮助，我们看一下面的代码：
```dart
ConstrainedBox(
    constraints: BoxConstraints(minWidth: 60.0, minHeight: 100.0),  //父
    child: UnconstrainedBox( //“去除”父级限制
      child: ConstrainedBox(
        constraints: BoxConstraints(minWidth: 90.0, minHeight: 20.0),//子
        child: redBox,
      ),
    )
)
```
上面代码中，如果没有中间的UnconstrainedBox，那么根据上面所述的多重限制规则，那么最终将显示一个90×100的红色框。但是由于 UnconstrainedBox “去除”了父ConstrainedBox的限制，则最终会按照子ConstrainedBox的限制来绘制redBox，即90×20：


但是，UnconstrainedBox对父限制的“去除”并非是真正的去除，上面例子中虽然红色区域大小是90×20，但上方仍然有80的空白空间。也就是说父限制的minHeight(100.0)仍然是生效的，只不过它不影响最终子元素的大小，但仍然还是占有相应的空间，可以认为此时的父ConstrainedBox是作用于子ConstrainedBox上，而redBox只受子ConstrainedBox限制，这一点请读者务必注意。

那么有什么方法可以彻底去除父BoxConstraints的限制吗？答案是否定的！所以在此提示读者，在定义一个通用的widget时，如果对子widget指定限制时一定要注意，因为一旦指定限制条件，子widget如果要进行相关自定义大小时将可能非常困难，因为子widget在不更改父widget的代码的情况下无法彻底去除其限制条件。

## DecoratedBox

DecoratedBox可以在其子widget绘制前(或后)绘制一个装饰Decoration（如背景、边框、渐变等）。DecoratedBox定义如下：
```dart
const DecoratedBox({
  Decoration decoration,
  DecorationPosition position = DecorationPosition.background,
  Widget child
})
```
- decoration：代表将要绘制的装饰，它类型为Decoration，Decoration是一个抽象类，它定义了一个接口 createBoxPainter()，子类的主要职责是需要通过实现它来创建一个画笔，该画笔用于绘制装饰。
- position：此属性决定在哪里绘制Decoration，它接收DecorationPosition的枚举类型，该枚举类两个值：
- background：在子widget之后绘制，即背景装饰。
- foreground：在子widget之上绘制，即前景。
## BoxDecoration
我们通常会直接使用BoxDecoration，它是一个Decoration的子类，实现了常用的装饰元素的绘制。
```dart
BoxDecoration({
  Color color, //颜色
  DecorationImage image,//图片
  BoxBorder border, //边框
  BorderRadiusGeometry borderRadius, //圆角
  List<BoxShadow> boxShadow, //阴影,可以指定多个
  Gradient gradient, //渐变
  BlendMode backgroundBlendMode, //背景混合模式
  BoxShape shape = BoxShape.rectangle, //形状
})
```
各个属性名都是自解释的，详情读者可以查看API文档，我们看一个示例：
```dart
 DecoratedBox(
    decoration: BoxDecoration(
      gradient: LinearGradient(colors:[Colors.red,Colors.orange[700]]), //背景渐变
      borderRadius: BorderRadius.circular(3.0), //3像素圆角
      boxShadow: [ //阴影
        BoxShadow(
            color:Colors.black54,
            offset: Offset(2.0,2.0),
            blurRadius: 4.0
        )
      ]
    ),
  child: Padding(padding: EdgeInsets.symmetric(horizontal: 80.0, vertical: 18.0),
    child: Text("Login", style: TextStyle(color: Colors.white),),
  )
)
```
![](https://cdn.jsdelivr.net/gh/flutterchina/flutter-in-action@1.0/docs/imgs/image-20180910115903588.png)

怎么样，通过BoxDecoration，我们实现了一个渐变按钮的外观，但此示例还不是一个标准的按钮，因为它还不能响应点击事件，我们将在本章末尾来实现一个完整的GradientButton。