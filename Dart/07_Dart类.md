>Dart是一种面向对象的语言，具有类和基于mixin的继承。每个对象都是一个类的实例，所有的类都是Object的子类。基于mixin的继承意味着，尽管每个类(除了Object)都只有一个超类，但类主体可以在多个类层次结构中重用。
##基础
### 使用类的成员（变量和方法）
```Dart
var p = Point(2, 2);
// 设置实例变量的值 y.
p.y = 3;
// 为避免最左操作数为空时出现异常，使用 ?.代替.
p?.y = 4;
// 获取变量的值
assert(p.y == 3);
// 调用方法
num distance = p.distanceTo(Point(4, 4));

```

### 使用构造函数
您可以使用构造函数创建一个对象。构造函数名可以是ClassName或ClassName.identifier。
```Dart
var p1 = Point(2, 2);
var p2 = Point.fromJson({'x': 1, 'y': 2});

// 下面的代码具有相同的效果，但是在构造函数名之前使用可选的new关键字
// 在Dart2中new关键字为可选关键字
var p1 = new Point(2, 2);
var p2 = new Point.fromJson({'x': 1, 'y': 2});

// 有些类提供常量构造函数。要使用常量构造函数创建编译时常量，请将const关键字放在构造函数名之前:
var p = const ImmutablePoint(2, 2);

// 构造两个相同的编译时常量会生成一个单一的、规范的实例:
var a = const ImmutablePoint(1, 1);
var b = const ImmutablePoint(1, 1);
assert(identical(a, b)); // They are the same instance!


// 在常量上下文中，可以在构造函数或文字之前省略const。例如，看看这个代码，它创建了一个const的 map集合:
const pointAndLine = const {
  'point': const [const ImmutablePoint(0, 0)],
  'line': const [const ImmutablePoint(1, 10), const ImmutablePoint(-2, 11)],
};

//除了第一次使用const关键字之外其他的const都可以省略：
const pointAndLine = {
  'point': [ImmutablePoint(0, 0)],
  'line': [ImmutablePoint(1, 10), ImmutablePoint(-2, 11)],
};

```
### 获得对象的类型
要在运行时获得对象类型，可以使用对象的runtimeType属性。

```Dart
print('The type of a is ${a.runtimeType}');
```

### 实例变量
```Dart
// 所有未初始化的实例变量都具有null值。
class Point {
  num x; // Declare instance variable x, initially null.
  num y; // Declare y, initially null.
  num z = 0; // Declare z, initially 0.
}

// 所有实例变量都生成隐式getter方法。非最终实例变量也生成隐式setter方法。
class Point {
  // 如果在声明实例变量的地方(而不是在构造函数或方法中)初始化实例变量，则在创建实例时(在构造函数及其初始化列表执行之前)设置该值。
  num x = 2;
  num y;
}

void main() {
  var point = Point();
  point.x = 4; // Use the setter method for x.
  assert(point.x == 4); // Use the getter method for x.
  assert(point.y == null); // Values default to null.
}

```

## 构造函数
通过创建一个与类同名的函数来声明构造函数(另外，还可以像[命名构造函数]中描述的一样选择一个附加标识符)。构造函数最常见的应用形式是使用构造函数生成一个类的新实例：
```Dart
class Point {
  num x, y;

  Point(num x, num y) {
    // There's a better way to do this, stay tuned.
    this.x = x;
    this.y = y;
  }
}
```
this关键字是指当前实例。

> 注意:只有在名称冲突时才使用它。否则，Dart的代码风格需要省略this

使用构造函数的参数为实例复制的使用非常常见，Dart具有语法上的优势，使这种使用更容易实现：
```Dart
class Point {
  num x, y;

  // Syntactic sugar for setting x and y
  // before the constructor body runs.
  Point(this.x, this.y);
}
```

### 默认构造函数
如果不声明构造函数，则为您提供默认构造函数。默认构造函数没有参数，并在超类中调用无参数构造函数。

### 构造函数不是继承
子类不从父类继承构造函数。没有声明构造函数的子类只有默认的构造函数（没有参数，没有名称）而不是从父类继承的构造函数。

### 命名的构造函数
使用命名构造函数可以在一个类中定义多个构造函数，或者让一个类的作用对于开发人员来说更清晰：
```Dart
class Point {
  num x, y;

  Point(this.x, this.y);

  // Named constructor
  Point.origin() {
    x = 0;
    y = 0;
  }
}
```
一定要记住构造函数是不会从父类继承的，这意味着父类的命名构造函数子类也不会继承。如果你希望使用在超类中定义的命名构造函数来创建子类，则必须在子类中实现该构造函数。

### 调用非默认的超类构造函数
默认情况下，子类中的构造函数调用父类的未命名的无参数构造函数。父类的构造函数在构造函数体的开始处被调用。如果类中有使用初始化列表，初始化列表将在调用超类之前执行。综上所述，执行顺序如下:

- 初始化列表
- 超类中的无参数构造函数
- main类中的无参数构造函数

如果超类没有未命名的无参数构造函数，则必须手动调用超类中的一个构造函数。在冒号(:)之后，在构造函数体(如果有的话)之前指定超类构造函数。

在下例中，Employee类的构造函数中调用了他的超类Person中的命名构造函数。
```Dart
class Person {
  String firstName;

  Person.fromJson(Map data) {
    print('in Person');
  }
}

class Employee extends Person {
  // Person does not have a default constructor;
  // you must call super.fromJson(data).
  Employee.fromJson(Map data) : super.fromJson(data) {
    print('in Employee');
  }
}

main() {
  var emp = new Employee.fromJson({});

  // Prints:
  // in Person
  // in Employee
  if (emp is Person) {
    // Type check
    emp.firstName = 'Bob';
  }
  (emp as Person).firstName = 'Bob';
}
```
```Shell
///结果输出为
in Person
in Employee
```
因为父类构造函数的参数是在调用构造函数之前执行的，所以参数可以是表达式，比如函数调用:
```Dart
class Employee extends Person {
  Employee() : super.fromJson(getDefaultData());
  // ···
}
```
> 警告：在超类的构造函数的参数中不能使用this关键字。例如，参数可以调用static方法但是不能调用实例方法

### 初始化列表
除了调用超类构造函数之外，还可以在构造函数主体运行之前初始化实例变量。初始值设定项用逗号分开。
```Dart
// Initializer list sets instance variables before
// the constructor body runs.
Point.fromJson(Map<String, num> json)
    : x = json['x'],
      y = json['y'] {
  print('In Point.fromJson(): ($x, $y)');
}
```
> 警告:初始化器的右边部分中无法访问this关键字。

在开发期间，可以通过在初始化列表中使用assert来验证输入。
```Dart
Point.withAssert(this.x, this.y) : assert(x >= 0) {
  print('In Point.withAssert(): ($x, $y)');
}
```
初始化列表在设置final字段时很方便。下面的示例初始化初始化列表中的三个final字段：

```Dart
import 'dart:math';

class Point {
  final num x;
  final num y;
  final num distanceFromOrigin;

  Point(x, y)
      : x = x,
        y = y,
        distanceFromOrigin = sqrt(x * x + y * y);
}

main() {
  var p = new Point(2, 3);
  print(p.distanceFromOrigin);
}
```
```Shell
///运行结果
3.605551275463989
```
### 重定向构造函数
有时，构造函数的唯一目的是重定向到同一个类中的另一个构造函数。重定向构造函数的主体为空，构造函数调用出现在冒号(:)之后。
```Dart
class Point {
  num x, y;

  // The main constructor for this class.
  Point(this.x, this.y);

  // Delegates to the main constructor.
  Point.alongXAxis(num x) : this(x, 0);
}
```
### 常量构造函数
如果您的类生成的对象不会改变，您可以使这些对象成为编译时常量。为此，定义一个const构造函数，并确保所有实例变量都是final的。
```Dart
class ImmutablePoint {
  static final ImmutablePoint origin =
      const ImmutablePoint(0, 0);

  final num x, y;

  const ImmutablePoint(this.x, this.y);
}
```
### 工厂构造函数
在实现构造函数时使用factory关键字，该构造函数并不总是创建类的新实例。例如，工厂构造函数可以从缓存返回实例，也可以返回子类型的实例。

以下示例演示工厂构造函数从缓存返回对象:
```Dart
class Logger {
  final String name;
  bool mute = false;

  // _cache is library-private, thanks to
  // the _ in front of its name.
  static final Map<String, Logger> _cache =
      <String, Logger>{};

  factory Logger(String name) {
    if (_cache.containsKey(name)) {
      return _cache[name];
    } else {
      final logger = Logger._internal(name);
      _cache[name] = logger;
      return logger;
    }
  }

  Logger._internal(this.name);

  void log(String msg) {
    if (!mute) print(msg);
  }
}
```
> 注意:工厂构造函数不能访问this关键字。

调用工厂构造函数，就像调用其他构造函数一样:
```Dart
var logger = Logger('UI');
logger.log('Button clicked');
```

## 方法
> 方法是为对象提供行为的函数。

### 实例方法
对象上的实例方法可以访问实例变量。下面示例中的distanceTo()方法是一个实例方法的示例:
```dart
import 'dart:math';

class Point {
  num x, y;

  Point(this.x, this.y);

  num distanceTo(Point other) {
    var dx = x - other.x;
    var dy = y - other.y;
    return sqrt(dx * dx + dy * dy);
  }
}
```
### Getters 和 setters 方法
getter和setter是对对象属性的读写访问的特殊方法。回想一下，每个实例变量都有一个隐式的getter，如果需要的话还可以加上一个setter。使用get和set关键字来实现getter和setter方法可以来读写其他属性：
```Dart
class Rectangle {
  num left, top, width, height;

  Rectangle(this.left, this.top, this.width, this.height);

  // Define two calculated properties: right and bottom.
  num get right => left + width;
  set right(num value) => left = value - width;
  num get bottom => top + height;
  set bottom(num value) => top = value - height;
}

void main() {
  var rect = Rectangle(3, 4, 20, 15);
  assert(rect.left == 3);
  rect.right = 12;
  assert(rect.left == -8);
}
```
使用getter和setter，你可以使用方法包装实例变量，而无需改动业务代码。

>注意:诸如increment(++)之类的操作符以预期的方式工作，无论getter是否被显式定义。为了避免任何意外的副作用，操作符只调用getter一次，将其值保存在一个临时变量中。

### 抽象方法
实例方法、getter和setter方法可以是抽象方法，之定义一个接口但是将具体实现留给其他类。抽象方法只能存在于抽象类中，抽象方法是没有方法体的。
```dart
abstract class Doer {
  // Define instance variables and methods...

  void doSomething(); // Define an abstract method.
}

class EffectiveDoer extends Doer {
  void doSomething() {
    // Provide an implementation, so the method is not abstract here...
  }
}
```
调用抽象方法会导致运行时错误。

## 抽象类
使用abstract修饰符定义不能实例化的抽象类。抽象类对于定义接口非常有用。如果您希望抽象类看起来是可实例化的，请定义一个工厂构造函数。

抽象类通常有抽象方法。这里有一个声明抽象类的例子，它有一个抽象的方法:
```Dart
// This class is declared abstract and thus
// can't be instantiated.
abstract class AbstractContainer {
  // Define constructors, fields, methods...

  void updateChildren(); // Abstract method.
}
```

## 隐式接口
每个类都隐式地定义一个接口，该接口包含类的所有实例成员及其实现的任何接口。如果您想创建一个类A，它支持类B的API而不继承B的实现，那么类A应该实现B接口。
```Dart
// A person. The implicit interface contains greet().
class Person {
  // In the interface, but visible only in this library.
  final _name;

  // Not in the interface, since this is a constructor.
  Person(this._name);

  // In the interface.
  String greet(String who) => 'Hello, $who. I am $_name.';
}

// An implementation of the Person interface.
class Impostor implements Person {
  get _name => '';

  String greet(String who) => 'Hi $who. Do you know who I am?';
}

String greetBob(Person person) => person.greet('Bob');

void main() {
  print(greetBob(Person('Kathy')));
  print(greetBob(Impostor()));
}
```
这里有一个例子，说明一个类实现多个接口:
```Dart
class Point implements Comparable, Location {...}
```

## 继承
使用extend创建子类，使用super引用超类:
```Dart
class Television {
  void turnOn() {
    _illuminateDisplay();
    _activateIrSensor();
  }
  // ···
}

class SmartTelevision extends Television {
  void turnOn() {
    super.turnOn();
    _bootNetworkInterface();
    _initializeMemory();
    _upgradeApps();
  }
  // ···
}
```

## 重写类的成员
子类可以覆盖实例方法、getter和setter。您可以使用@override注释来指示你重写了某个成员方法:
```Dart
class SmartTelevision extends Television {
  @override
  void turnOn() {...}
  // ···
}
```
要在类型安全的代码中缩小方法参数或实例变量的类型，可以使用covariant关键字。

### 重写操作符
您可以重写下表中显示的操作符。例如，如果定义一个Vector类，可以定义一个+方法来让两个向量相加。

`<`	|`+`|	`|`	|`[]`
--|--|--|--
`>`	|`/`|	`^`|	`[]=`
`<=`|	`~/`|	`&`|	`~`
`>=`|	`*`	|`<<`|	`==`
`-`	|`%`	|`>>`|
下例在类中重写了+和-操作符：
```Dart
class Vector {
  final int x, y;

  Vector(this.x, this.y);

  Vector operator +(Vector v) => Vector(x + v.x, y + v.y);
  Vector operator -(Vector v) => Vector(x - v.x, y - v.y);

  // Operator == and hashCode not shown. For details, see note below.
  // ···
}

void main() {
  final v = Vector(2, 3);
  final w = Vector(2, 2);

  assert(v + w == Vector(4, 5));
  assert(v - w == Vector(0, 1));
}
```
如果重写`==`，还应该重写对象的hashCode getter。有关override == 和hashCode的示例，请参见[ Implementing map keys]。

有关重写的更多信息，请参见[扩展类]。

### noSuchMethod()
可以重写noSuchMethod()方法来处理程序访问一个不存在的方法或者成员变量：
```Dart
class A {
  // Unless you override noSuchMethod, using a
  // non-existent member results in a NoSuchMethodError.
  @override
  void noSuchMethod(Invocation invocation) {
    print('You tried to use a non-existent member: ' +
        '${invocation.memberName}');
  }
}
```
您不能调用未实现的方法，除非下列任何一个是正确的:

- 被调用者有静态方法dynamic
- 被调用者有一个静态类型来定义未实现的方法(抽象也可以OK)，而接收者的动态类型有一个noSuchMethod()的实现，它与类对象中的方法不同。

## 枚举类型
枚举类型，通常称为枚举或枚举类型，是一种特殊类型的类，用于表示固定数量的常量值。

### 使用枚举
使用enum关键字声明一个枚举类型：
```Dart
enum Color { red, green, blue }
```
枚举中的每个值都有一个索引getter，它返回enum声明中值的从0开始的位置。例如，第一个值有索引0，第二个值有索引1。
```Dart
assert(Color.red.index == 0);
assert(Color.green.index == 1);
assert(Color.blue.index == 2);
```
要获取枚举中所有值的列表，请使用enum的values 常量。
```Dart
List<Color> colors = Color.values;
assert(colors[2] == Color.blue);
```
您可以在switch语句中使用enum，如果switch的case不处理enum的所有值，将会报一个警告消息:
```dart
var aColor = Color.blue;

switch (aColor) {
  case Color.red:
    print('Red as roses!');
    break;
  case Color.green:
    print('Green as grass!');
    break;
  default: // Without this, you see a WARNING.
    print(aColor); // 'Color.blue'
}
```
枚举类型有以下限制:
- 不能子类化、混合或实现枚举。
- 不能显式实例化一个枚举

## 为类添加mixins特性
mixin是在多个类层次结构中重用类代码的一种方式。

要使用mixin，请在with关键字后面加上一个或多个mixin名称。下面的例子显示了两个使用mixin的类:
```Dart
class Musician extends Performer with Musical {
  // ···
}

class Maestro extends Person
    with Musical, Aggressive, Demented {
  Maestro(String maestroName) {
    name = maestroName;
    canConduct = true;
  }
}

// 要实现mixin，创建一个Object的子类，不声明构造函数，也不调用super。例如:

abstract class Musical {
  bool canPlayPiano = false;
  bool canCompose = false;
  bool canConduct = false;

  void entertainMe() {
    if (canPlayPiano) {
      print('Playing piano');
    } else if (canConduct) {
      print('Waving hands');
    } else {
      print('Humming to self');
    }
  }
}
```
## 静态变量和静态方法
使用static关键字实现类范围的变量和方法。

### 静态变量
静态变量(类变量)对于类范围内的状态和常量是有用的:
```dart
class Queue {
  static const initialCapacity = 16;
  // ···
}

void main() {
  assert(Queue.initialCapacity == 16);
}
```
静态变量在使用之前不会初始化。

> 注意:此页面遵循代码样式规范，对常量名使用小驼峰命名法。

### 静态方法
静态方法(类方法)不对实例进行操作，因此无法访问该实例。例如:
```Dart
import 'dart:math';

class Point {
  num x, y;
  Point(this.x, this.y);

  static num distanceBetween(Point a, Point b) {
    var dx = a.x - b.x;
    var dy = a.y - b.y;
    return sqrt(dx * dx + dy * dy);
  }
}

void main() {
  var a = Point(2, 2);
  var b = Point(4, 4);
  var distance = Point.distanceBetween(a, b);
  assert(2.8 < distance && distance < 2.9);
  print(distance);
}
```
>注意:对于通用或广泛使用的实用程序和功能，考虑使用顶级函数，而不是静态方法。

可以使用静态方法作为编译时常量。例如，可以将静态方法作为参数传递给常量构造函数。
