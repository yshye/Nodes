## 介绍
Container 容器控件，包含一个子Widget，具备alignment、padding等属性，方便布局摆放child

## 类结构
![](img/14_1.png)

## 属性
### child
> 子控件

### alignment
> 对齐方式
```dart
 /// The top left corner.
  static const Alignment topLeft = Alignment(-1.0, -1.0);
  /// The center point along the top edge.
  static const Alignment topCenter = Alignment(0.0, -1.0);
  /// The top right corner.
  static const Alignment topRight = Alignment(1.0, -1.0);
  /// The center point along the left edge.
  static const Alignment centerLeft = Alignment(-1.0, 0.0);
  /// The center point, both horizontally and vertically.
  static const Alignment center = Alignment(0.0, 0.0);
  /// The center point along the right edge.
  static const Alignment centerRight = Alignment(1.0, 0.0);
  /// The bottom left corner.
  static const Alignment bottomLeft = Alignment(-1.0, 1.0);
  /// The center point along the bottom edge.
  static const Alignment bottomCenter = Alignment(0.0, 1.0);
  /// The bottom right corner.
  static const Alignment bottomRight = Alignment(1.0, 1.0);
```
### color  和 decoration
> 设置背景色或背景
* 两者无法共存
```dart
assert(color == null || decoration == null,
    'Cannot provide both a color and a decoration\n'
    'The color argument is just a shorthand for "decoration: new BoxDecoration(color: color)".'
),
decoration = decoration ?? (color != null ? BoxDecoration(color: color) : null),
```


### foregroundDecoration
> 前景设置

### margin
> 边距设置

### constraints
> 布局约束

### transform
> 矩阵变换

