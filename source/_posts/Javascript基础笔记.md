---
title: JavaScript基础笔记
date: 2017-03-12 01:04:40
tags: [JS, 笔记]
---

# 基本语法

Javascript区分大小写。

标识符命名规则：
- 第一个字符必须是一个字母、下划线` _ `或一个美元符号` $ `
- 其他字符可以是字母、下划线、美元符号或数字
- 不能把关键字、保留字、true、false和null作为标识符

|ECMAScript全部关键字|
|---------|--------|-------|------------|---------|
| break   |  case  | catch |  continue  | default |
| delete  |   do   |  else |   finally  |   for   |
|function |   if   |   in  | instanceof |   new   |
| return  | switch |  this |    throw   |   try   |
| typeof  |  var   |  void |    while   |   with  |

|ECMA-262第三版全部保留字|
|---------|--------|-------|------------|---------|
|abstract |boolean |byte |char |class |const |debugger |
|double |enum |export |extends |final |float |goto |
|implements |import |int |interface |long |native |
|package |private |protected |public |short |static |
|super |synchronized |throws |transient |volatile |

注释：`//`单行注释 | `/* */`多行注释

Javascript的每个语句以`;`结束，语句块用`{}`。
虽然Javascript不强制使用`;`，不过始终补上`;`是一个良好的习惯。
可选分号通用规则：如果当前语句和下一行语句无法合并解析，Javascrpt则在第一行后填补分号。
示例：
```
var a
a
=
3
console.log(a)
```
等价于
```
var a;
a = 3;
console.log(a);
```
两个例外：
1. 如果`return`、`break`和`continue`后紧跟着换行，则会在换行处填补分号。
	示例：
	```
    return
    true
    ```
    等价于
    ```
    return;
    true;
    ```
2. 如果将`++`或`--`用做后缀表达式，应当和表达式在同一行，否则行尾自动填补分号。
	示例：
    ```
    x
    ++
    y
    ```
    等价于
    ```
    x;
    ++y;
    ```

赋值语句：
```
var x = 0;
```

字符串也可以视为完整的语句
```
"Hello World!"
```
注意如果字符串不用`''`或`""`包裹会抛出`ReferenceError（引用错误）`。

# 数据类型

## 原始类型（primitive type）

- Number
    包含两种数值：整数和浮点数。
    八进制数值字面量：前导为`0`
    十六进制数值字面量：前导为`0x`
    浮点类型数值包含一个小数点，且小数点后面必须至少有一位数字。由于保存浮点数值需要的内存空间比整数型数值大两倍，如果浮点数值小数点后没有数字或数字都为0会自动转化为整型。
    对于过大或过小的数值可以用科学表示法`e表示法`，用e表示该数值的前面数值的10的指数次幂。浮点数值的范围在：`Number.MIN_VALUE`到`Number.MAX_VALUE`之间。如果超过了浮点数值范围的最大值或最小值，则会出现`Infinity（正无穷）`或`-Infinity（负无穷）`。
    `isFinite()`函数可以确定一个数值是否超过了规定范围，没有超过返回`true`，超过返回`false`。
    `NaN`即Not a Number，用于表示一个本来要返回数值的操作数未返回数值的情况（这样就不回抛出错误）。`NaN`与任何值都不相等，包括它自身。
    判别一个字面量x是否为`NaN`有两种方式：`x != x` 或者 `isNaN(x)` 。注意，`isNaN(true)`会返回`false`，因为`true`会被转换为数字1。
    非数值转化为数值：`Number()`、`parseInt()`、`parseFloat()`
- String
    用于表示由零或多个16位Unicode字符组成的字符串。
- Boolean
    有两个值：`true`和`false`。而`true`不一定等于1，`false`不一定等于0.由于JavaScript区分大小写，所以`TRUE`和`FALSE`或其他不是Boolean类型的值。
    其他类型转换成Boolean类型规则：![其他类型转换成Boolean类型规则](http://ommn78ss1.bkt.clouddn.com/static/images/JavaScript%E5%9F%BA%E7%A1%80%E7%AC%94%E8%AE%B0/%E5%85%B6%E4%BB%96%E7%B1%BB%E5%9E%8B%E8%BD%AC%E6%8D%A2Boolean.png)
- NULL
    只用一个值`null`。表示一个空对象引用（指针）。使用`typeof`操作符检测`null`会返回`object`
- Undefined
    只有一个值`undefined`。在使用`var`声明变量但没有对其初始化时，这个变量的值就是`undefined`
    对于未初始化的变量与未声明的变量，使用`typeof`操作符时都是返回`undefined`。从逻辑上而言，前者的值是`undefined`，而后者是`ReferenceError（引用错误）`
## 对象类型（objext type）
- Array
- Object
原始值是 *不可更改* 的；原始值得比较是 *值* 的比较。
对象值是 *可修改* 的；对象的比较是 *引用* 的比较。

**typeof**操作符：用来检测变量的数据类型
