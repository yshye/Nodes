> 流程控制在大部分语言中大同小异，在Dart中只有最后一个assert比较特殊。
### 常用流程控制
- if 和 else
- for循环
- while和do-while循环
- break和continue
- switch和case
- assert

### if 和 else
> 与JavaScript不同的是，条件必须使用布尔值，不允许其他值。

### For循环
```Dart
// 基本For循环
var message = StringBuffer('Dart is fun');
for (var i = 0; i < 5; i++) {
  message.write('!');
}

// Dart for循环内部的闭包捕获了索引的值，避免了JavaScript中常见的陷阱。例如:
var callbacks = [];
for (var i = 0; i < 2; i++) {
  callbacks.add(() => print(i));
}
callbacks.forEach((c) => c());

```

### While 和 do-while 循环
```Dart
// while循环在循环之前先检验条件是否为真
while (!isDone()) {
  doSomething();
}

// do-while循环在一次循环结束之后检查条件是否为真
do {
  printLine();
} while (!atEndOfPage());
// 所以使用while和do-while要非常小心，因为使用do-while如果条件为假也会至少执行一次循环体中的语句。
```

### Break 和 continue

```Dart
// 使用break终止循环：
while (true) {
  if (shutDownRequested()) break;
  processIncomingRequests();
}

// 使用continue跳出本次循环继续下次循环：
for (int i = 0; i < candidates.length; i++) {
  var candidate = candidates[i];
  if (candidate.yearsExperience < 5) {
    continue;
  }
  candidate.interview();
}

```

### 可迭代的循环
```Dart
// 如果你使用的是可迭代的，比如列表或集合，你可能会用不同的方式来写这个例子:
candidates
    .where((c) => c.yearsExperience >= 5)
    .forEach((c) => c.interview());

class A {
  var yearsExperience;
  A(int year)
  {
    this.yearsExperience = year;
  }
  interview() {
    print(this.yearsExperience);
  }
}

void main() {
  List personList = [
    A(1),
    A(2),
    A(5),
    A(6)
  ];

  ///第一种写法
  for(int i = 0; i < personList.length; i++) {
    var person = personList[i];
    if (person.yearsExperience < 5) {
      continue;
    }

    person.interview();
  }

  ///第二种写法
  personList
      .where((c) => c.yearsExperience >= 5)
      .forEach((c) => c.interview());

}
```
>两次结果都相同，那么这儿的where()方法究竟是什么呢，我们在后续会接触到，他是dart.core中的iterable.dart中的方法，他返回复合where()中条件的集合视图，所以在where()后边我们可以继续调用forEach()，当然如果where()返回的集合视图为空也不会出现错误，仅仅是forEach()不执行而已。这儿where()的作用类似于过滤器。

### switch 和 case
在Dart中switch语句使用 “==”运算来比较整数，字符串或者编译时常量。被比较对象必须都是同一个类的实例(而不是它的任何子类型)，并且这个类不能重写“==”操作。枚举类型在switch语句是一种非常好的应用场景。

>注意:Dart中的Switch语句适用于有限的情况，例如在解释器或扫描器中。

switch的规则是每个非空的case子句以一个break语句结束。结束非空case子句的其他有效方法是continue、throw或return语句。

当没有case子句匹配时，使用default子句执行代码:
```Dart
var command = 'OPEN';
switch (command) {
  case 'CLOSED':
    executeClosed();
    break;
  case 'PENDING':
    executePending();
    break;
  case 'APPROVED':
    executeApproved();
    break;
  case 'DENIED':
    executeDenied();
    break;
  case 'OPEN':
    executeOpen();
    break;
  default:
    executeUnknown();
}
```
下面的示例省略了case子句中的break语句，从而产生错误:
```Dart
var command = 'OPEN';
switch (command) {
  case 'OPEN':
    executeOpen();
    // ERROR: Missing break

  case 'CLOSED':
    executeClosed();
    break;
}
```
然而，Dart支持空的case子句,支持fall-through的格式:
```Dart
var command = 'CLOSED';
switch (command) {
  case 'CLOSED': // Empty case falls through.
  case 'NOW_CLOSED':
    // 无论command是CLOSED还是NOW_CLOSED都执行
    executeNowClosed();
    break;
}
```
如果你真的需要使用fall-through格式，你可以使用continue语句和一个标签，例如：
```Dart
var command = 'CLOSED';
switch (command) {
  case 'CLOSED':
    executeClosed();
    continue nowClosed;
  // Continues executing at the nowClosed label.

  nowClosed:
  case 'NOW_CLOSED':
    // Runs for both CLOSED and NOW_CLOSED.
    executeNowClosed();
    break;
}
```
case子句可以有局部变量，只能在该子句的范围内可见。

### Assert (断言)
如果布尔条件为false，则使用assert语句中断正常执行。
> Assert语句不会影响生产环境中代码的执行，它仅仅在测试环境中起作用。在Flutter的调试模式下可以使用assert。默认情况下，像(dartdevc typically)只支持开发环境的工具默认支持assert。例如dart和dart2js通过命令行标记：--enable-asserts来支持asserts。

要将提示消息附加到断言，请添加一个字符串作为第二个参数。
```Dart
assert(urlString.startsWith('https'),
    'URL ($urlString) should start with "https".');
```
断言的第一个参数可以是任何解析为布尔值的表达式。如果表达式的值为true，则断言成功并继续执行。如果是false，则断言失败，并抛出异常(AssertionError)。
