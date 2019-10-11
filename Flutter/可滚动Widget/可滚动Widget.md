## 可滚动Widget简介
当内容超过显示窗口（ViewPort)时，如果没有特殊处理，Flutter会提示Overflow错误。为此，Flutter提供了多种可滚动Widget用于显示列表和长布局。如ListView、GridView等。
```dart
Scrollable({
    ...
    this.axisDirection = AxisDirection.down,
    this.controller,
    this.physics,
    @required this.viewportBuilder,// 后面介绍
})
```
- axisDirection：滚动方向。
- physics：此属性接受一个ScrollPhysics对象，它决定可滚动Widget如何响应用户操作。
- controller：此属性接受一个ScrollController对象。ScrollController的主要作用是控制滚动位置和监听滚动事件。

## Scrollbar
Scrollbar是一个Material风格的滚动指示器（滚动条），如果要给可滚动widget添加滚动条，只需要将Scrollbar作为可股东widget的服widget即可，如：
```dart
Scrollbar(
    child: SingleChildScrollView(
        ...
    ),
);
```
Scrollbar和CupertionScrollbar都是通过ScrollController来监听滚动时间来确定滚动条位置，关于ScrollController详细的内容我们将在后面专门一节介绍。

## CupertionScrollbar
CupertinoScrollbar是iOS风格的滚动条，如果你使用的是Scrollbar，那么在iOS平台它会自动切换为CupertinoScrollbar。

## ViewPort视口
在很多布局系统中都有ViewPort的概念，在Flutter中，术语ViewPort（视口），如无特别说明，则是指一个Widget的实际显示区域。例如，一个ListView的显示区域高度是800像素，虽然其列表项总高度可能远远超过800像素，但是其ViewPort仍然是800像素。

## 主轴和纵轴
在可滚动widget的坐标描述中，通常将滚动方向的陈为主轴，非滚动方向称为纵轴。
