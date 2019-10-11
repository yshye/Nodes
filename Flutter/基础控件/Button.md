## 介绍
> 在Material Widget 库中提供了多种按钮Widget，如RaisedButton、FlatButton、OutlineButton等。

所有Material库中的按钮都具备以下共同点：
1. 按下时会有“水波动画”。
2. 有一个onPressed属性来设置点击的回调，当按下时会执行回调，如果不提供该回调则按钮会处于禁用状态，不相应用户点击。

## RaisedButton
> 漂浮按钮，默认带有阴影、灰色背景，按下后，阴影变大。
### 定义
```dart
 const RaisedButton({
    Key key,
    @required VoidCallback onPressed, //按钮点击回调
    ValueChanged<bool> onHighlightChanged,
    ButtonTextTheme textTheme,
    Color textColor, //按钮文字颜色
    Color disabledTextColor, //按钮禁用时的文字颜色
    Color color,//按钮背景颜色
    Color disabledColor,//按钮禁用时的背景颜色
    Color highlightColor,//按钮按下时的背景颜色
    Color splashColor,//点击时，水波动画中水波的颜色
    Brightness colorBrightness, //按钮主题
    double elevation,//正常状态下的阴影
    double highlightElevation,//按下时的阴影
    double disabledElevation,// 禁用时的阴影
    EdgeInsetsGeometry padding, //按钮的填充
    ShapeBorder shape, //外形
    Clip clipBehavior = Clip.none,
    MaterialTapTargetSize materialTapTargetSize,
    Duration animationDuration,
    Widget child, //按钮的内容
  })
```
### 简单样例
```dart
RaisedButton(
    onPressed: () => print("你点击了RaisedButton"),
    child: Text("RaisedButton"),
)
```

## FlatButton
> 扁平按钮，默认背景透明并不带阴影，按下后，会有背景色。
### 定义
```dart
const FlatButton({
    Key key,
    @required VoidCallback onPressed,//按钮点击回调
    ValueChanged<bool> onHighlightChanged,
    ButtonTextTheme textTheme,
    Color textColor,//按钮文字颜色
    Color disabledTextColor, //按钮禁用时的文字颜色
    Color color,//按钮背景颜色
    Color disabledColor,//按钮禁用时的背景颜色
    Color highlightColor,//按钮按下时的背景颜色
    Color splashColor,//点击时，水波动画中水波的颜色
    Brightness colorBrightness,//按钮主题，默认是浅色主题 
    EdgeInsetsGeometry padding,//按钮的填充
    ShapeBorder shape,//外形
    Clip clipBehavior = Clip.none,
    MaterialTapTargetSize materialTapTargetSize,
    @required Widget child,//按钮的内容
  })
```
### 样例
```dart
FlatButton(
    onPressed: () => print("你点击了FlatButton"),
    child: Text("FlatButton"),
)
```

## OutlineButton
> OutlineButton默认有一个边框，不带阴影且背景透明。按下后，边框颜色会变亮、同时出现背景和阴影(较弱)：
### 定义
```dart
const OutlineButton({
    Key key,
    @required VoidCallback onPressed, 
    ButtonTextTheme textTheme,
    Color textColor,
    Color disabledTextColor,
    Color color,//按钮背景颜色
    Color highlightColor,//按钮按下时的背景颜色
    Color splashColor,//点击时，水波动画中水波的颜色
    double highlightElevation,
    this.borderSide,
    this.disabledBorderColor,
    this.highlightedBorderColor,
    EdgeInsetsGeometry padding,
    ShapeBorder shape,
    Clip clipBehavior = Clip.none,
    Widget child,
  })
```

### 样例
```dart
OutlineButton(
    onPressed: () => print("你点击了OutlineButton"),
    child: Text("OutlineButton"),
)
```

## IconButton
> 可点击的Icon，不包含文字，默认没有背景，点击会出现背景

### 定义
```dart
 const IconButton({
    Key key,
    this.iconSize = 24.0,// icon大小
    this.padding = const EdgeInsets.all(8.0),
    this.alignment = Alignment.center,// 默认居中
    @required this.icon,// 内部的icon
    this.color,//按钮背景颜色
    this.highlightColor,//按钮按下时的背景颜色
    this.splashColor, //点击时，水波动画中水波的颜色
    this.disabledColor,// 禁用颜色
    @required this.onPressed,// 点击回调
    this.tooltip // 长按回调
  })
```

### 样例

```dart
IconButton(
    onPressed: () => print("你点击了IconButton"),
    tooltip:"你长按了我",
    icon: Icon(Icons.thumb_up),
)
```
## 自定义按钮样式
> 按钮外观可以通过其属性来定义，不同按钮属性大同小异。

### 自定义一个圆角蓝色按钮
```dart
FlatButton(
    color
)
```



