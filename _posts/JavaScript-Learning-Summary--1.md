---
title: JavaScript学习总结(一)----基础知识
date: 2016-4-5 20:22:33
tags:
  - JavaScript
  - 学习总结
  -
categories: JavaScript
---

## 什么是JavaScript？
JavaScript一种直译式脚本语言，是一种动态类型、弱类型、基于原型的语言。简单地说，它是一种运行在浏览器中的解释型的编程语言。同时也是世界上最流行的脚本语言。
JavaScript是一种弱类型语言，JavaScript 会忽略多余的空格。您可以向脚本添加空格，来提高其可读性。
JavaScript中的一切（包括变量，函数名和操作符）都区分大小写。

<!-- more -->

## JavaScript的运行机制
JavaScript在执行前，同一个作用域的代码会先解释再执行，浏览器引擎会自动扫描带var关键字和带function关键字声明的变量和定义的函数，这个过程称为预解释。

### var关键字预解释
先看如下代码：

```js
alert(n);//弹出undefined
var n = 10;
```

为什么结果是undefind？原因是代码执行前n会被声明，但是不会被赋值，相当于以下代码：

```js
var n;
alert(n);//弹出undefined
n = 10;
```

然后看以下代码：

```js
alert(n);
n = 10;// 会报错 n is not defined
```

这次会报错的原因就是因为在代码运行时，并没有声明这个变量n，所以会报错。

### function关键字预解释

再看下如下代码：

```js
fn();//弹出hello
function fn(){
    alert('hello');
}
```

执行结果弹出hello，fn能够正常执行，原因是在代码执行前fn被预解释了，在预解释时已经将fn定义（defined）了

>所以得到如下结论：var用来声明变量，function用来定义函数，只不过function关键字声明和定义函数是同时执行的，而var它只能声明变量，并不具备定义的功能。

### 这算bug吗？（火狐除外）

```js
alert(n);
fn();
if(false) {
    var n = 10;
    function fn(){
        alert('hello');
    }
}
```

第一行代码执行会弹出undefined，第二行代码执行会弹出hello；是因为n和fn在代码执行前被预解释了，即使if条件判断为false，执着的浏览器引擎也会将带var关键字声明的变量n和带function关键定义的fn扫描到。

### 变量只能声明一次

看如下代码：

```js
alert(n); // 弹出undefind
var n = 10;
var n = 9;
var n;
alert(n); // 弹出9
```

相当于如下代码：

```js
var n;
alert(n); // 弹出undefind
n = 10;
n = 9;
alert(n); // 弹出9
```

所以第一次会弹出undefind，而第二次会弹出9。
再看如下代码，分析结果：

```js
fn(); // 弹出2
function fn() {
    alert('1');
}
fn(); // 弹出2
function fn() {
    alert('2');
}
fn(); // 弹出2
```

前面的function会被后面的function重定义，所以三次调用`fn()`的结果都是弹出2

## 标识符

所谓标识符，就是指变量、函数、属性的名字，或者函数的参数。

### 命名规则

> - 标识符必须是以字母、下划线、美元号等符号开头，不能以数字开头。其后可以是零个或若干个字母、数字、下划线、美元等符号组成。
> - 标识符名称对大小写敏感（y 和 Y 是不同的变量）
> - 标识符不能为关键字，也不能为保留字（虽然暂时可能不会出现问题，但不保证以后不会作为关键字使用）

### 关键字和保留字

以下是ECMAScript的全部关键字（带*号上标的是第5 版新增的关键字）：

|   break   |    do    | instanceof | typeof |
| :-------: | :------: | :--------: | :----: |
|   case    |   else   |    new     |  var   |
|   catch   | finally  |   return   |  void  |
| continue  |   for    |   switch   | while  |
| debugger* | function |    this    |  with  |
|  default  |    if    |   throw    | delete |
|    in     |   try    |            |        |

ECMA-262 还描述了另外一组不能用作标识符的保留字。尽管保留字在这门语言中还没有任何特定的用途。但它们有可能在将来被用作关键字。以下是ECMA-262 第3 版定义的全部保留字：

| abstract |    enum    |    int    |    short     |
| :------: | :--------: | :-------: | :----------: |
| boolean  |   export   | interface |    static    |
|   byte   |  extends   |   long    |    super     |
|   char   |   final    |  native   | synchronized |
|  class   |   float    |  package  |    throws    |
|  const   |    goto    |  private  |  transient   |
| debugger | implements | protected |   volatile   |
|  double  |   import   |  public   |              |

第5 版把在非严格模式下运行时的保留字缩减为下列这些：

| class |  enum  | extends | super |
| :---: | :----: | :-----: | :---: |
| const | export | import  |       |

在严格模式下，第5 版还对以下保留字施加了限制：

| implements | package | public | interface |
| :--------: | :-----: | :----: | :-------: |
|  private   | static  |  let   | protected |
|   yield    |         |        |           |

## 变量

ECMAScript 的变量（Variables）是松散类型的，所谓松散类型就是可以用来保存任何类型的数据。
变量命名都以类型前缀+有意义的单词组成，用驼峰式命名法增加变量和函式的可读性。例如：sUserName，nCount。

### 前缀规范（匈牙利命名法）：

每个局部变量都需要有一个类型前缀，按照类型可以分为：

>s：表示字符串(String)。例如：sName，sHtml；
>n：表示数字(Number)。例如：nPage，nTotal；
>b：表示逻辑(Boolean)。例如：bChecked，bHasLogin；
>a：表示数组(Array)。例如：aList，aGroup；
>r：表示正则表达式(Regular Expression)。例如：rDomain，rEmail；
>f：表示函数(Function)。例如：fGetHtml，fInit；
>o：表示以上未涉及到的其他对象(Object)，例如：oButton，oDate；
>g：表示全局变量(Global Variable)，例如：gUserName，gLoginTime；

### 命名规则

> - 如果声明了一个变量但没有对其赋值，则其值为undefined。
> - 一个好的编程习惯是，在代码开始处，统一对需要的变量进行声明。
> - 相邻的两个变量可以省略var用逗号相连

### 变量思维导图

![](/images/2016/04/1.gif)

## 数据类型
### 5种基本数据类型
#### Undefind类型

Undefind类型只有一个值，即特殊的undefind。是被用来形容未经初始化的变量，而后面的Null类型被用来形容空对象指针。

- 在使用var声明变量，但未对其加以初始化时，这个变量的类型就是Undefined，且其默认初始化值为undefined。

```js
var message;
alert(typeof message)//undefind
alert(message)//undefind
```

等价于下面代码

```js
var message = undefind;
alert(typeof message)//undefind
alert(message)//undefind
```

>一般而言，不存在需要显式地把一个变量设置为undefind值得情况，字面量undefind的主要作用是用于比较。这个值的存在是为了正式区分空对象指针与未经初始化的变量。

- 对未声明与初始化的变量，直接使用，那么这个变量的类型也是Undefined，但是没有默认初始化值。

```js
//var age;
alert(typeof age)//undefind
alert(age)//报错
```

>对于尚未经声明过的变量，只能执行一项操作，即使用typeof操作符监测其数据类型。
>事实证明，对于未初始化和为声明的变量指向typeof操作符都返回了undefind值

#### Null类型

Null类型是第二个只有一个值的数据类型，这个特殊的值是unll。从逻辑角度来看， null 值表
示一个空对象指针，而这也正是使用 typeof 操作符检测 null 值时会返回 "object" 的原因。

- 如果定义的变量准备在将来用于保存对象，那么最好将该变量初始化为 null 而不是其他值。这样
  一来，只要直接检查 null 值就可以知道相应的变量是否已经保存了一个对象的引用，如下面的例子
  所示：

```js
if (car != null){
// 对 car 对象执行某些操作
}
```

>实际上， undefined 值是派生自 null 值的，因此 ECMA-262规定对它们的相等性测试要返回 true ：

```js
alert(null == undefined); //true
```

- 如前所述，无论在什么情况下都没有必要把一个变量的值显式地设置为 undefined ，可是同样的规则对 null 却不适用。换句话说，只要意在保存对象的变量还没有真正保存对象，就应该明确地让该变量保存 null 值。这样做不仅可以体现 null 作为空对象指针的惯例，而且也有助于进一步区分 null 和 undefined 。

#### Boolean类型

Boolean 类型是 ECMAScript 中使用得最多的一种类型，该类型只有两个字面值： true 和 false 。

> - 需要注意的是， Boolean 类型的字面值 true 和 false 是区分大小写的。也就是说， True 和 False（以及其他的混合大小写形式）都不是 Boolean 值，只是标识符。
> - 虽然 Boolean 类型的字面值只有两个，但 ECMAScript 中所有类型的值都有与这两个 Boolean 值等价的值。要将一个值转换为其对应的 Boolean 值，可以调用转型函数 Boolean()

#### Number类型
该类型的表示方法有两种形式，第一种是整数，第二种为浮点数。

1.整数：

- 可以通过十进制，八进制，十六进制的字面值来表示。

```js
var intNum=55;//十进制
var octalNum=070;//八进制数，第一位必须是0，解析为56
var  octalNum=079;//无效的8进制数，9超过了8进制数的范围，解析为79
var hexNum=0xA;//十六进制数，前两位必须是0x，后跟16进制数字（0~9及A~F）
```

2.浮点数值：

- 所谓浮点数值，就是该数值中必须包含一个小数点，并且小数点后面必须至少有一位数字。虽然小数点前面可以没有整数，但不推荐。
- 由于保存浮点数值需要的内存空间是保存整数值的两倍，因此 ，如果小数点后面没有跟任何数字，那么这个数值就可以作为整数值来保存。
- 极大或极小的数值，可以用 e 表示法（即科学计数法）表示的浮点数值表示。

3.数值范围：

- ECMAScript 能够表示的最小数值和最大数值分别保存在 Number.MIN_VALUE和Number.MAX_VALUE 中。
- 超出 JavaScript 数值范围的值，将被自动转换成特殊的 Infinity 值。包含 -Infinity （负无穷）和 Infinity （正无穷）。
- JavaScript的内置函数isFinite() 函数可以用于检查其参数是否是无穷大。如果其参数是有限数字（或可转换为有限数字），那么返回 true。否则，如果其参数是 NaN（非数字），或者是正、负无穷大的数，则返回 false。

4.NaN

- NaN ，即非数值（Not a Number）是一个特殊的数值，这个数值用于表示一个本来要返回数值的操作数未返回数值的情况（这样就不会抛出错误了）。
- 任何涉及 NaN 的操作（例如 NaN /10）都会返回 NaN。
- NaN 与任何值都不相等，包括 NaN 本身。所以要判断某个值是否是 NaN，不能使用 == 或 === 运算符。
- JavaScript的内置函数isNaN() 函数可以用于检查其参数是否是非数字值。如果其参数是特殊的非数字值 NaN（或者能被转换为这样的值），返回的值就是 true。如果 其参数是其他值,则返回 false。

5.数值转换

- 有三个函数可以把非数值转换为数值：Number()，parseInt()，parseFloat()。第一个可以用于任何数据类型，后两个，则专门用于把字符串转化为数值。
- 这三个函数对于同样的输入有不同的效果。
- [数值转换]()（先挖个坑，另写一篇文章）

#### String类型

String类型即字符串。可以由双引号（"）或单引号（'）表示，至于用单引号，还是双引号，在JavaScript中是没有差别的。
- toString()方法，可以将一个值转换为一个字符串，数值，布尔值，对象和字符串值都有toString()方法，但null和undefind值没有这个方法。

###两种复杂数值类型

## 参考来源
[JavaScript学习总结（一）基础部分](https://segmentfault.com/a/1190000000652749)
[网易前端JavaScript编码规范](http://www.php100.com/html/webkaifa/javascript/2012/0616/10550.html)
[JS关键字和保留字汇总](http://www.itxueyuan.org/view/6627.html)
