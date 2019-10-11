## 介绍
> 沿着水平或垂直方向排列Widget。

## Column&Row
> 垂直和水平线性布局

```dart
Column({
    Key key,
    MainAxisAlignment mainAxisAlignment = MainAxisAlignment.start,// 表示子Widgets在Column所占用的水平空间内对齐方式
    MainAxisSize mainAxisSize = MainAxisSize.max,//在垂直方向上的占据空间
    CrossAxisAlignment crossAxisAlignment = CrossAxisAlignment.center,// 表示子Widgets在纵轴方向的对齐方式
    TextDirection textDirection,// 水平布局的顺序（从左到右、从右到左）
    VerticalDirection verticalDirection = VerticalDirection.down,// 表示Column纵轴（垂直）的对齐方向
    TextBaseline textBaseline,
    List<Widget> children = const <Widget>[],// 子控件
  }) 
```
- textDirection：表示水平方向子widget的布局顺序(是从左往右还是从右往左)，默认为系统当前Locale环境的文本方向(如中文、英语都是从左往右，而阿拉伯语是从右往左)。
- mainAxisSize：表示Row在主轴(水平)方向占用的空间，默认是MainAxisSize.max，表示尽可能多的占用水平方向的空间，此时无论子widgets实际占用多少水平空间，Row的宽度始终等于水平方向的最大宽度；而MainAxisSize.min表示尽可能少的占用水平空间，当子widgets没有占满水平剩余空间，则Row的实际宽度等于所有子widgets占用的的水平空间；
- mainAxisAlignment：表示子Widgets在Row所占用的水平空间内对齐方式，如果mainAxisSize值为MainAxisSize.min，则此属性无意义，因为子widgets的宽度等于Row的宽度。只有当mainAxisSize的值为MainAxisSize.max时，此属性才有意义，MainAxisAlignment.start表示沿textDirection的初始方向对齐，如textDirection取值为TextDirection.ltr时，则MainAxisAlignment.start表示左对齐，textDirection取值为TextDirection.rtl时表示从右对齐。而MainAxisAlignment.end和MainAxisAlignment.start正好相反；MainAxisAlignment.center表示居中对齐。读者可以这么理解：textDirection是mainAxisAlignment的参考系。
- verticalDirection：表示Row纵轴（垂直）的对齐方向，默认是VerticalDirection.down，表示从上到下。
- crossAxisAlignment：表示子Widgets在纵轴方向的对齐方式，Row的高度等于子Widgets中最高的子元素高度，它的取值和MainAxisAlignment一样(包含start、end、 center三个值)，不同的是crossAxisAlignment的参考系是verticalDirection，即verticalDirection值为VerticalDirection.down时crossAxisAlignment.start指顶部对齐，verticalDirection值为VerticalDirection.up时，crossAxisAlignment.start指底部对齐；而crossAxisAlignment.end和crossAxisAlignment.start正好相反；
- children ：子Widgets数组。