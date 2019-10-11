## 介绍
> ListView是最常用的可滚动widget，它可以沿一个方向线性排布所有子widget。

```dart
ListView({
    Key key,
     //可滚动widget公共参数
    Axis scrollDirection = Axis.vertical,
    bool reverse = false,
    ScrollController controller,
    bool primary,
    ScrollPhysics physics,
    EdgeInsetsGeometry padding,
    // ListView各个构造函数的共同参数 
    this.itemExtent,
    bool shrinkWrap = false,
    bool addAutomaticKeepAlives = true,
    bool addRepaintBoundaries = true,
    bool addSemanticIndexes = true,
    double cacheExtent,
    // 子widget列表
    List<Widget> children = const <Widget>[],
    int semanticChildCount,
  })
```

## 常用属性
**childrenDelegate**：自定义子模型时用到。

**itemExtent**：Item 的范围，scrollDirection 为 Axis.vertical 时限制高度，scrollDirection 为 Axis.horizontal 限制宽度。

**cacheExtent**：预加载的区域。

**controller**：滑动监听，值为一个 ScrollController 对象，这个属性应该可以用来做下拉刷新和上垃加载，后面详细研究。

**padding**：整个 ListView 的内间距。

**physics**：设置 ListView 如何响应用户的滑动行为，值为一个 ScrollPhysics 对象，它的实现类常用的有：
    AlwaysScrollableScrollPhysics：总是可以滑动。
    NeverScrollableScrollPhysics：禁止滚动。
    BouncingScrollPhysics：内容超过一屏，上拉有回弹效果。
    ClampingScrollPhysics：包裹内容，不会有回弹，感觉跟 AlwaysScrollableScrollPhysics 差不多。

**primary**：是否是与 PrimaryScrollController 关联的主滚动视图，若为 true 则 controller 必须为空。  

**reverse**：Item 的顺序是否反转，若为 true 则反转。

**scrollDirection**：ListView 的方向，为 Axis.vertical 表示纵向，为 Axis.horizontal 表示横向。

**shrinkWrap**：该属性表示是否根据子widget的总长度来设置ListView的长度，默认值为false 。默认情况下，ListView的会在滚动方向尽可能多的占用空间。当ListView在一个无边界(滚动方向上)的容器中时，shrinkWrap必须为true。

**addAutomaticKeepAlives**：该属性表示是否将列表项（子widget）包裹在AutomaticKeepAlive widget中；典型地，在一个懒加载列表中，如果将列表项包裹在AutomaticKeepAlive中，在该列表项滑出视口时该列表项不会被GC，它会使用KeepAliveNotification来保存其状态。如果列表项自己维护其KeepAlive状态，那么此参数必须置为false。

**addRepaintBoundaries**：该属性表示是否将列表项（子widget）包裹在RepaintBoundary中。当可滚动widget滚动时，将列表项包裹在RepaintBoundary中可以避免列表项重绘，但是当列表项重绘的开销非常小（如一个颜色块，或者一个较短的文本）时，不添加RepaintBoundary反而会更高效。和addAutomaticKeepAlive一样，如果列表项自己维护其KeepAlive状态，那么此参数必须置为false。

## 默认构造函数
默认构造函数有一个children参数，它接受一个Widget列表（List）。这种方式适合只有少量的子widget的情况，因为这种方式需要将所有children都提前创建好（这需要做大量工作），而不是等到子widget真正显示的时候再创建。实际上通过此方式创建的ListView和使用SingleChildScrollView+Column的方式没有本质的区别。下面是一个例子：

```dart
ListView(
  shrinkWrap: true, 
  padding: const EdgeInsets.all(20.0),
  children: <Widget>[
    const Text('I\'m dedicating every day to you'),
    const Text('Domestic life was never quite my style'),
    const Text('When you smile, you knock me out, I fall apart'),
    const Text('And I thought I was so smart'),
  ],
);
```
> *注意：可滚动widget通过一个List来作为其children属性时，只适用于子widget较少的情况，这是一个通用规律，并非ListView自己的特性，像GridView也是如此。

## ListView.builder
> 适合列表项多（无限)的情况，因为只有当Widget真正显示的时候才会被创建

### 核心属性
- **itemBuilder** 列表构造器，类型为IndexedWidgetBuilder，返回一个Widget。当列表滚动到具体的index位置时，会调用该构造器构造列表项。
- **itemCount** 列表项的数量，如果为null，则是无限列表。
### 示例
```dart
ListView.builder(
    itemCount: 100,
    itemExtent: 50,
    itemBuilder: (BuildContext context, int index) {
    return ListTile(
        title: Text('$index'),
    );
    },
);
```

## ListView.separated
> 可以生成列表项之间的分割器，它除了比ListView.builder多了一个separatorBuilder参数，该参数是一个分割器生成器。

### 示例
> 奇数行添加一条blue色下划线，偶数行添加一条pink色下划线。

