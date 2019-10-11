##介绍
> 类似Android中的ScrollView，只接收一个子Widget
## 定义
```dart
const SingleChildScrollView({
    Key key,
    this.scrollDirection = Axis.vertical, //滚动方向，默认是垂直方向
    this.reverse = false, // 是否按照阅读方向相反的方向滑动
    this.padding,
    bool primary, //指是否使用widget树中默认的PrimaryScrollController
    this.physics,
    this.controller,
    this.child,
  })
```
## 示例
```dart
Scrollbar(
    child: SingleChildScrollView(
    padding: EdgeInsets.all(16.0),
    child: Center(
        child: Column(
        // 动态创建一个List<Widget>
        children: str
            .split("")
            // 每个字母都可以是一个Text显示，字体是原来的2倍
            .map((c) => Text(c, textScaleFactor: 2.0))
            .toList(),
        ),
    ),
    ),
);
```


