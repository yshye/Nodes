> Flutter 可以使用Image加载显示asset、本地、内存以及网络图片

## ImageProvider
> 是一个抽象类，定义了图片数据获取的接口load()，不同的数据源由对应不同的ImageProvider，如AssertImage、NetworkImage

## Image

### 类结构
```dart
 const Image({
    Key key,
    @required this.image, // 图片源
    this.semanticLabel,
    this.excludeFromSemantics = false,
    this.width,//图片的宽
    this.height, //图片高度
    this.color,//图片的混合色值
    this.colorBlendMode,//混合模式
    this.fit,//缩放模式
    this.alignment = Alignment.center, //对齐方式
    this.repeat = ImageRepeat.noRepeat,//重复方式
    this.centerSlice,// 内部拉伸区域
    this.matchTextDirection = false,
    this.gaplessPlayback = false,
    this.filterQuality = FilterQuality.low,
  })
```
### 构造方法
| 方式          | 解释                                                              |
| ------------- | ----------------------------------------------------------------- |
| Image()       | 通用方法，使用ImageProvider实现，如下方法本质上也是使用的这个方法 |
| Image.asset   | 加载资源图片                                                      |
| Image.file    | 加载本地图片文件                                                  |
| Image.network | 加载网络图片                                                      |
| Image.memory  | 加载Uint8List资源图片                                             |

### 属性
#### image
> 抽象类，需要自己实现获取图片数据的操作

常用ImageProvider
- ExactAssetImage
- AssetImage
- NetworkImage
- FileImage
- MemoryImage

#### width & height
> 显示区域的宽和高。

#### fit
> 缩放模式

- BoxFit.fill 全图显示，拉伸、充满
- BoxFit.contain 原比例全图显示
- BoxFit.cover 拉伸、裁剪、充满
- BoxFit.fitWidth 宽度充满
- BoxFit.fitHeight 高度充满
- BoxFit.scaleDown 类似contain，但是不允许显示超过源图片大小，可变小，不可变大
- BoxFit.none 原图显示

#### color & colorBlendMode
> 混合模式，配合使用

#### alignment
> 控制图片的摆放位置

#### repeat
> 重复显示
- ImageRepeat.repeat X、Y方向都重复显示
- ImageRepeat.repeatX 横向重复
- ImageRepeat.repeatY 竖向重复
- ImageRepeat.none 不重复
  
#### centerSlice
> 当图片被拉伸时，centerSlice定义的区域会被拉升
```dart
Image image = Image.asset(
    'image/14v.png',
    width: 300.0,
    height: 300.0,
    fit: BoxFit.contain,
    centerSlice: Rect.fromCircle(
        center: const Offset(100.0,100.0),
        radius: 10.0,
    ),
)
```
```dart
assert(sourceSize == inputSize, 'centerSlice was used with a BoxFit that does not guarantee that the image is fully visible.');
```
`* 当显示比例小于原图片大小时，会报错！`

#### matchTextDirection
> 与 Directionality 配合使用

#### gaplessPlayback
> 当ImageProvider发生变化后，重新加载图片的过程中，原图片的展示是否保留。若值为true，保留，若为false，不保留，直接空白等待下一张图片加载。

## Icon
> Flutter中可以像Web开发一样使用iconfont,即字体图标，它是将图标做成字体文件，然后通过指定不同的字符而显示不同的图片。

Icon与Image相比有如下优势
1. 体积小
2. 矢量化，放大不变形。
3. 可以应用文本样式。
4. 可以利用TextSpan和文本混用。

### 使用Material Design字体图标
Flutter默认包含了一套Material Design的字体图标，在pubspec.yaml文件中的配置如下：
```dart
 flutter:
   user-material-design: true
```
所有的图标集合可查看官网 http://material.io/tools/icons
