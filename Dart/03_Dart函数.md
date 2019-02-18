## 定义函数
函数也是对象，当没有指定返回值的时候，函数返回null。函数定义方法如下：
```Dart
String sayHello(String name)
{
  return 'Hello $name!';
}

//is  is!操作符判断对象是否为指定类型，如num、String等
assert(sayHello is Function);
```
由于类型可选，所以也可以写成：
```Dart
sayHello(name)
{
  return 'Hello $name!';
}
```
如果函数只是简单的返回一个表达式的值，可以使用箭头语法 =>expr;
```Dart
sayHello(name) => 'Hello $name!';
```
Dart中匿名函数的写法 (name)=>’Hello $name!’;
```Dart
var  sayHello = (name)=>'Hello $name!';
```

## 函数闭包
```Dart
Function makeSubstract(num n)
{
  return (num i) => n - i;
}

void main()
{
  var x = makeSubstract(5);
  print(x(2));
}
```
`Dart中函数也是对象`
```Dart
var callbacks = [];
for (var i = 0; i < 3; i++) {
  // 在列表 callbacks 中添加一个函数对象，这个函数会记住 for 循环中当前 i 的值。
  callbacks.add(() => print('Save $i'));
}
callbacks.forEach((c) => c()); // 分别输出 0 1 2
```

## 可选参数
Dart中支持两种可选参数：命名可选参数和位置可选参数
但两种可选不能同时使用
- 命名可选参数使用大括号`{}`，默认值用冒号`:`
```Dart
FunX(a, {b, c:3, d:4, e})
{
  print('$a $b $c $d $e');
}
```
- 位置可选参数使用方括号`[]`，默认值用等号`=`

在位置可选参数的函数中，大括号内的参数可以指定0个或多个在调用的时候参数值会依次按顺序赋值。
```Dart
FunY(a, [b, c=3, d=4, e])
{
  print('$a $b $c $d $e');
}

void main()
{
  FunX(1, b:3, d:5);
  FunY(1, 3, 5);
}
```
