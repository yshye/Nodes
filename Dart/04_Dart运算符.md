### 按照运算符优先级高低进行排序

描述|操作符
--|--
一元后置操作符|	expr++    expr--    ()    []    .    ?.
一元前置操作符|	-expr    !expr    ~expr    ++expr    --expr
乘除运算|	*    /    %    ~/
加减运算|	+    -
移位运算|	<<    >>
按位与|	&
按位异或|	^
按位或|	`|`
关系和类型测试|	>=    >    <=    <    as    is    is!
相等|	==    ！=
逻辑与|	&&
逻辑或|	`||`
是否为null|	??
天健判断（三元运算）|	expr1 ? expr2 : expr3
级联|	..
赋值|	=    *=    /=    ~/=    %=    +=    -=    <<=    >>=    &=    ^=   
> 当使用操作符后，就变成了表达式。

### 算术运算符

运算符|说明
--|--
+ | 加法
- | 减法
-expr | 一元减号，也称为否定
*  | 乘法
/ | 除法
~/  | 取模运算
%  | 取余运算

```Dart
assert(2 + 3 == 5);
assert(2 - 3 == -1);
assert(2 * 3 == 6);
assert(5 / 2 == 2.5); // Result is a double
assert(5 ~/ 2 == 2); // Result is an int
assert(5 % 2 == 1); // Remainder

assert('5/2 = ${5 ~/ 2} r ${5 % 2}' == '5/2 = 2 r 1');


// Dart还支持前缀和后缀递增和递减运算符。
var a, b;

a = 0;
b = ++a; // Increment a before b gets its value.
assert(a == b); // 1 == 1

a = 0;
b = a++; // Increment a AFTER b gets its value.
assert(a != b); // 1 != 0

a = 0;
b = --a; // Decrement a before b gets its value.
assert(a == b); // -1 == -1

a = 0;
b = a--; // Decrement a AFTER b gets its value.
assert(a != b); // -1 != 0
```

### 相等和关系运算符

运算符|说明
--|--
==|	相等
!=|	不等
>|	大于
<|	小于
>=|	大于等于
<=|	小于等于

### 类型测试操作符
> a, is, and is!操作符可以方便地在运行时检查类型。

运算符|说明
--|--
as|	形态转换
is|	如果对象具有指定的类型，则为True
is!|	如果对象具有指定的类型，则为False

### 赋值运算符
> ??=操作符仅仅在变量为null时会赋值。未初始化和后来手动赋值为null的情况都会执行此操作赋值。

```Dart
// Assign value to a
a = value;
// Assign value to b if b is null; otherwise, b stays the same
// 仅仅在b为空的情况下b被赋值value否则b的值不变
b ??= value;
```

### 复合赋值操作符
> 如+=将操作与赋值合并。

`=`	|`-=`|	`/=`|	`%=`|	`>>=`|	`^=`
--|--|--|--|--|--
`+=`|	`*=`|	`~/=`|	`<<=`|	`&=`|	`|=`

### 逻辑运算符
> 可以使用逻辑运算符组合布尔表达式或取反布尔表达式。

运算符	|说明
--|--
`!expr` |	对!后的表达式结果取反(如果表达式结果为false则表达式前加！使其变为true，反之亦然)
`||`	|逻辑或
`&&`	|逻辑与（且）

### 位和移位运算
运算符	|说明
--|--
`&`	|按位与
`|`	|按位或
`^`	|按位异或
`~expr`	|按位取反
`<<`	|左移
`>>`	|右移

```Dart
final value = 0x22;
final bitmask = 0x0f;

assert((value & bitmask) == 0x02); // AND
assert((value & ~bitmask) == 0x20); // AND NOT
assert((value | bitmask) == 0x2f); // OR
assert((value ^ bitmask) == 0x2d); // XOR
assert((value << 4) == 0x220); // Shift left
assert((value >> 4) == 0x02); // Shift right
```
### 条件表达式
```Dart
// 如果条件为真，则计算expr1(并返回其值);否则，计算并返回expr2的值。
condition ? expr1 : expr2

// 如果expr1是非空的，则返回其值;否则，计算并返回expr2的值。
expr1 ?? expr2

// 基于布尔表达式的结果选择赋值
var visibility = isPublic ? 'public' : 'private';

// 布尔表达式只想判断值是否为null
String playerName(String name) => name ?? 'Guest';

```

### 级联表示法
> `..` 允许您在同一个对象上创建一个操作序列。

### 其它运算符
运算符	|名称|	说明
--|--|--
`()`|	功能函数|	表示一个函数调用
`[]`|	访问列表|	引用列表中指定索引处的值
`·`	|访问成员|	表示表达式的属性;例如:foo.bar从表达式foo中选择属性bar，如果foo为null，会抛出异常
`?.`	|根据条件访问成员|	和(.)相似，但是左边的操作数可以为空；例如： foo?.bar 从foo的表达式中选择bar属性，如果foo为空则返回null
