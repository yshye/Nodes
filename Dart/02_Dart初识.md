# 02_Dart初识
## Dart简介
>   Dart是谷歌开发的计算机编程语言，后来被Ecma (ECMA-408)认定为标准。它被用于web、服务器、移动应用和物联网等领域的开发。它是宽松开源许可证（修改的BSD证书）下的开源软件。

>   Dart是面向对象的、类定义的、单继承的语言。它的语法类似C语言，可以转译为JavaScript，支持接口(interfaces)、混入(mixins)、抽象类(abstract classes)、具体化泛型(reified generics)、可选类型(optional typing)和sound type system。

## Hello Word
```dart
void main()
{
  print("Hello, world!");
}
```
## Dart语法规则
- Dart中所有的变量包括数字、函数和null 都是对象，每一个 对象都是一个类的实例，他们都继承于Object
- Dart是强类型语言，但是生明变量时也可以不指定类型，因为Dart可以自动推断，在上面的例子中，变量number就被自动推断为int类型
- Dart支持泛型，如`List<int>`表示集合元素类型为整型、`List<dynamic>`表示元素类型为任何类型
- Dart除了支持我们常见的静态函数(类直接调用)和普通函数(对象调用)外，同时也支持顶级函数如main() 和嵌套函数(函数里面的函数，也叫本地函数)
- 与函数类似，Dart也支持顶级变量，同时支持静态变量和实例变量，实例变量也被叫做字段或属性
和java不同，Dart没有public、protected、private权限修饰符，在Dart里以下划线`_`开头的标志符表示是私有的
- Dart里面的标识符可以字母或下划线(_)开头，以字符和数字的任何组合。
- 要注意区分表达式和语句的不同，如var a = 3 + 2; //整体是一个赋值语句，“=”右边部分即"3 + 2"是一个表达式。
- Dart工具会向你发出两种类型的提醒：警告和错误。警告代表你的代码可能有问题，但是不会阻止程序的运行；错误分为编译错误和运行错误，前者会阻止程序的运行，后者则会在程序运行使抛出异常！

## 关键字
![](/img/keywords.png)
> - 上面带有字样`1`的是内置标志符号，这些不能作为变量名。
> - 带有上标`2`的单词是内置的标识符。为了简化将JavaScript代码移植到DART的任务，这些关键字在大多数地方都是有效的标识符，但它们不能用作类名或类型名称，也不能用作导入前缀。
> - 带有`3`字样的是Dart2新增的用于支持异步的关键字，其他的都是保留字！

## 变量
### 变量声明有以下几种方式：
```dart
var name = 'Bob'; //类型自动推断
dynamic name = 'Bob';//dynamic表示变量类型不是单一的
String name = 'Bob';//明确声明变量的类型
int lineCount;//所有的变量包括数字类型，如果没有初始化，其默认值都是null
```
### const和final
用法和其他语言类似,在声明变量的时候，除了var，还可以使用const和final,同时，在使用const和final的时候，可以省略var或者其他类型。

const和final定义的都是常量，值不能改变并且在声明的时候就必须初始化但是也有细微差别，简单来说
- const定义的是编译时常量，只能用编译时常量来初始化
- final定义的常量可以用变量来初始化
```Dart
final time = new DateTime.now(); //Ok
const time = new DateTime.now(); //Error，new DateTime.now()不是const常量
```
var、final在左边定义变量的时候，并不关心右边是不是常量,但是如果右边用了const，那么不管左边如何，右边都必须是常量。
```Dart
const list = const[1,2,3];//Ok
const list = [1,2,3];//Error

final list = [1,2,3];//Ok
final list = const[1,2,3];//Ok
final list = const[new DateTime.now(),2,3];//Error,const右边必须是常量
```

## 数据类型
Dart中所有东西都是对象，包括数字、函数等
它们都继承自Object，并且默认值都是null（包括数字）因此数字、字符串都可以调用各种方法

Dart有七种内置的数据类型:
- numbers
- strings
- booleans
- lists (也称为数组)
- maps
- runes (用于表示字符串中的Unicode字符)
- symbols

### numbers
> Dart中Numbers有两种形式
1. int
> 整数值不大于64位，取决于平台。在DART VM上，值范围为 -2^63 to 2^63 - 1.编译为JavaScript的DART使用JavaScript数字，允许从-2^53到2^53-1之间的值。
2. double
> 64位(双精度)浮点数字

int和double都是num的子类型，num包含基本操作符如+ - / *，同时也有众多的方法如abs() ceil() floor()等，int里面还有位操作符如>>,更多详细内容参见[dart:math](https://api.dartlang.org/stable/dart-math).

```Dart
void main()
{
  //Dart 语言本质上是动态类型语言，类型是可选的
  //可以使用 var 声明变量，也可以使用类型来声明变量
  //一个变量也可以被赋予不同类型的对象
  //但大多数情况，我们不会去改变一个变量的类型

  //字符串赋值的时候，可以使用单引号，也可以使用双引号
  var str1 = "Ok?";

  //如果使用的是双引号，可以内嵌单引号
  //当然，如果使用的是单引号，可以内嵌双引号，否则需要“\”转义
  //String str2 = ‘It\’s ok!’;
  String str2 = "It's ok!";

  //使用三个单引号或者双引号可以多行字符串赋值
  var str3 = """Dart Lang
  Hello,World!""";

  //在Dart中，相邻的字符串在编译的时候会自动连接
  //这里发现一个问题，如果多个字符串相邻，中间的字符串不能为空，否则报错
  //但是如果单引号和双引号相邻，即使是空值也不会报错，但相信没有人这么做
  //var name = 'Wang''''Jianfei'; 报错
  var name = 'Wang'' ''Jianfei';

  //assert 是语言内置的断言函数，仅在检查模式下有效
  //如果断言失败则程序立刻终止
  assert(name == "Wang Jianfei");

  //Dart中字符串不支持“+”操作符，如str1 + str2
  //如果要链接字符串，除了上面诉说，相邻字符串自动连接外
  //还可以使用“$”插入变量的值
  print("Name：$name");

  //声明原始字符串，直接在字符串前加字符“r”
  //可以避免“\”的转义作用，在正则表达式里特别有用
  print(r"换行符：\n");

  //Dart中数值是num，它有两个子类型：int 和 double
  //int是任意长度的整数，double是双精度浮点数
  var hex = 0xDEADBEEF;

  //翻了半天的文档，才找打一个重要的函数：转换进制，英文太不过关了
  //上面提到的字符串插值，还可以插入表达式：${}
  print("整型转换为16进制：$hex —> 0x${hex.toRadixString(16).toUpperCase()}");

}
```
### Booleans
Dart是强布尔类型检查，只有当值是true是才为真，其他都是false，声明时用bool

### Lists
在 Dart　语言中，具有一系列相同类型的数据被称为 List 对象。
Dart List 对象类似JavaScript 语言的 array 对象。
```Dart
var list = [1, 2, 3];//分析器自动推断list为List<int>,所以里面不能加入其他类型的元素
print(list.length);//3
list[1] = 11;
print(list.toString());//[1, 11, 3]
var constantList = const [1, 2, 3];
// constantList[1] = 1; //因为前面的赋值使用了const 所以这句会报错.
constantList= [5];//这样可以
```
### Maps
map是一个包含key和value的对象，key不能重复
```Dart
var gifts = {
  // Key:    Value
  'first': 'partridge',
  'second': 'turtledoves',
  'fifth': 'golden rings'
};

var nobleGases = {
  2: 'helium',
  10: 'neon',
  18: 'argon',
};

var gifts2 = new Map();
gifts['first'] = 'partridge';
gifts['second'] = 'turtledoves';
gifts['fifth'] = 'golden rings';

var nobleGases2 = new Map();
nobleGases[2] = 'helium';
nobleGases[10] = 'neon';
nobleGases[18] = 'argon';

//获取value
print(gifts2['first']);// partridge;
print(gifts2.length);// 3
```
### Runes
Dart 中 使用runes 来获取UTF-32字符集的字符。String的 codeUnitAt and codeUnit属性可以获取UTF-16字符集的字符
```Dart
var clapping = '\u{1f44f}';
print(clapping);
print(clapping.codeUnits);
print(clapping.runes.toList());

Runes input = new Runes(
  '\u2665  \u{1f605}  \u{1f60e}  \u{1f47b}  \u{1f596}  \u{1f44d}');
print(new String.fromCharCodes(input));
```
### Symbols
symbol字面量是编译时常量，在标识符前面加#。如果是动态确定，则使用Symbol构造函数，通过new来实例化.我们可能永远也不用不到Symbol
```Dart
print(#s == new Symbol('s'));//true
```
