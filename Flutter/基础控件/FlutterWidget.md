## 介绍
Flutter提供了一套丰富、强大的基础widget，在基础widget库之上Flutter又提供了一套Material风格（Android默认的视觉风格）和一套Cupertino风格（iOS视觉风格）的widget库。要使用基础widget库，需要先导入：
```dart
import 'package:flutter/widgets.dart';
```

## 基础widget
- `Text`：该 widget 可让您创建一个带格式的文本。
- `Row`、 `Column`： 这些具有弹性空间的布局类Widget可让您在水平（Row）和垂直（Column）方向上创建灵活的布局。其设计是基于web开发中的Flexbox布局模型。
- `Stack`： 取代线性布局 (译者语：和Android中的FrameLayout相似)，Stack允许子 widget 堆叠， 你可以使用 Positioned 来定位他们相对于Stack的上下左右四条边的位置。Stacks是基于Web开发中的绝对定位（absolute positioning )布局模型设计的。
- `Container`：可让您创建矩形视觉元素。container 可以装饰一个BoxDecoration, 如 background、一个边框、或者一个阴影。 Container 也可以具有边距（margins）、填充(padding)和应用于其大小的约束(constraints)。另外，  Container可以使用矩阵在三维空间中对其进行变换。

## 什么是 Material Design 和 Flutter Material 组件？
Material Design 意在为你构建一个大胆而且美观的数字产品设计系统。将风格、品牌、交互、动效通过统一的准则结合，发掘产品最大的设计潜力。

**Flutter 的 Material 组件（MDC - Flutter）** 通过在应用间和平台间提供一个统一的用户体验组件库，把设计和工程合二为一。秉承着 Google 的前端开发标准，Material Design 系统正在向多端一致体验、像素级完美呈现的方向发展。Material Design 组件（MDC）也同样适用于 Android、iOS 和 Web。

## Material widget
Flutter提供了一套丰富的Material widget，可帮助您构建遵循Material Design的应用程序。Material应用程序以MaterialApp widget开始， 该widget在应用程序的根部创建了一些有用的widget，比如一个Theme，它配置了应用的主题。 是否使用MaterialApp完全是可选的，但是使用它是一个很好的做法。在之前的示例中，我们已经使用过多个Material widget了，如：Scaffold、AppBar、FlatButton等。要使用Material widget，需要先引入它：
```dart
import 'package:flutter/material.dart';
```

## Cupertino widget
Flutter也提供了一套丰富的Cupertino风格的widget，尽管目前还没有Material widget那么丰富，但也在不断的完善中。值得一提的是在Material widget库中，有一些widget可以根据实际运行平台来切换表现风格，比如MaterialPageRoute，在路由切换时，如果是Android系统，它将会使用Android系统默认的页面切换动画(从底向上)，如果是iOS系统时，它会使用iOS系统默认的页面切换动画（从右向左）。由于在前面的示例中还没有Cupertino widget的示例，我们实现一个简单的Cupertino页面：
```dart
//导入cupertino widget库
import 'package:flutter/cupertino.dart';

class CupertinoTestRoute extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return CupertinoPageScaffold(
      navigationBar: CupertinoNavigationBar(
        middle: Text("Cupertino Demo"),
      ),
      child: Center(
        child: CupertinoButton(
            color: CupertinoColors.activeBlue,
            child: Text("Press"),
            onPressed: () {}
        ),
      ),
    );
  }
}
```


## Widget与Element
>Widget的功能是“描述一个UI元素的配置数据”,Flutter中真正代表屏幕上显示元素的类是Element。
- Widget实际上就是Element的配置数据，Widget树实际上是一个配置树，而真正的UI渲染树是由Element构成；不过，由于Element是通过Widget生成，所以它们之间有对应关系，所以在大多数场景，我们可以宽泛地认为Widget树就是指UI控件树或UI渲染树。
- 一个Widget对象可以对应多个Element对象。这很好理解，根据同一份配置（Widget），可以创建多个实例（Element）。

## 主要接口
```dart
@immutable
abstract class Widget extends DiagnosticableTree {
  const Widget({ this.key });
  final Key key;

  @protected
  Element createElement();

  @override
  String toStringShort() {
    return key == null ? '$runtimeType' : '$runtimeType-$key';
  }

  @override
  void debugFillProperties(DiagnosticPropertiesBuilder properties) {
    super.debugFillProperties(properties);
    properties.defaultDiagnosticsTreeStyle = DiagnosticsTreeStyle.dense;
  }

  static bool canUpdate(Widget oldWidget, Widget newWidget) {
    return oldWidget.runtimeType == newWidget.runtimeType
        && oldWidget.key == newWidget.key;
  }
}
```
- Widget类继承自`DiagnosticableTree`，DiagnosticableTree即“诊断树”，主要作用是提供调试信息。
- `Key`: 这个key属性类似于React/Vue中的key，主要的作用是决定是否在下一次build时复用旧的widget，决定的条件在`canUpdate()`方法中。
- `createElement()`：正如前文所述“一个Widget可以对应多个Element”；Flutter Framework在构建UI树时，会先调用此方法生成对应节点的Element对象。此方法是Flutter Framework隐式调用的，在我们开发过程中基本不会调用到。
- `debugFillProperties(...)` 复写父类的方法，主要是设置诊断树的一些特性。
- `canUpdate(...)`是一个静态方法，它主要用于在Widget树重新build时复用旧的widget，其实具体来说，应该是：是否用新的Widget对象去更新旧UI树上所对应的Element对象的配置；通过其源码我们可以看到，只要newWidget与oldWidget的runtimeType和key同时相等时就会用newWidget去更新Element对象的配置，否则就会创建新的Element。

## StatelessWidget
> StatelessWidget用于不需要维护状态的场景，它通常在build方法中通过嵌套其它Widget来构建UI，在构建过程中会递归的构建其嵌套的Widget。
```dart
class Echo extends StatelessWidget {
  const Echo({
    Key key,  
    @required this.text,
    this.backgroundColor:Colors.grey,
  }):super(key:key);

  final String text;
  final Color backgroundColor;

  @override
  Widget build(BuildContext context) {
    return Center(
      child: Container(
        color: backgroundColor,
        child: Text(text),
      ),
    );
  }
}
```

## StatefulWidget
> 具有可变状态(`state`)的`Widget`.
```dart
abstract class StatefulWidget extends Widget {
  const StatefulWidget({ Key key }) : super(key: key);

  @override
  StatefulElement createElement() => new StatefulElement(this);

  @protected
  State createState();
}
```
- `StatefulElement` 间接继承自`Element`类，与`StatefulWidget`相对应（作为其配置数据）。`StatefulElement`中可能会多次调用`createState()`来创建状态(`State`)对象。
- `createState()` 用于创建和Stateful widget相关的状态，它在Stateful widget的生命周期中可能会被多次调用。例如，当一个Stateful widget同时插入到widget树的多个位置时，Flutter framework就会调用该方法为每一个位置生成一个独立的`State`实例，其实，本质上就是一个`StatefulElement`对应一个`State`实例。

## State
> 一个StatefulWidget类会对应一个State类，State表示与其对应的StatefulWidget要维护的状态。
1. 在widget build时可以被同步读取。
1. 在widget生命周期中可以被改变，当`State`被改变时，可以手动调用其`setState()`方法通知Flutter framework状态发生改变，Flutter framework在收到消息后，会重新调用其build方法重新构建widget树，从而达到更新UI的目的。

### 常用属性
- widget
  >它表示与该State实例关联的widget实例，由Flutter framework动态设置。注意，这种关联并非永久的，因为在应用声明周期中，UI树上的某一个节点的widget实例在重新构建时可能会变化，但State实例只会在第一次插入到树中时被创建，当在重新构建时，如果widget被修改了，Flutter framework会动态设置State.widget为新的widget实例。
- context
  >它是BuildContext类的一个实例，表示构建widget的上下文，它是操作widget在树中位置的一个句柄，它包含了一些查找、遍历当前Widget树的一些方法。每一个widget都有一个自己的context对象。

### 生命周期
![](img/state_life.png)

大致可以看成三个阶段
1. 初始化（插入渲染树）
2. 状态改变（在渲染树中存在）
3. 销毁（从渲染树中移除）

### 主要函数
- initState
  > 当插入渲染树的时候调用，这个函数在生命周期中只调用一次。这里可以做一些初始化工作，比如初始化State的变量。
- didChangeDependencies
  > 这个函数会紧跟在initState之后调用，并且可以调用BuildContext.inheritFromWidgetOfExactType
- didUpdateWidget
  > 当组件的状态改变的时候就会调用didUpdateWidget,比如调用了setState.
- deactivate
  > 在dispose之前，会调用这个函数。
- dispose
  > 一旦到这个阶段，组件就要被销毁了，这个函数一般会移除监听，清理环境。