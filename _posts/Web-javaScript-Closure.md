title: 《你不知道的javaScript》上卷 · 闭包
date: 2016-03-02 15:32:03
tags: Web
---

> 为了理解闭包这个问题我们按如下的思路来进行，什么闭包，为什么会产生闭包，闭包这种特性有什么用？

+ 什么是闭包
    - 所谓闭包就是函数在其被定义的作用域之外执行时仍可访问该作用域的变量的特性。利用这个特性我们可以访问其他作用域内部变量
    - 简单的说，闭包就是函数的嵌套，内层的函数可以使用外层函数的所有变量，即使外层函数已经执行完毕（这点涉及JavaScript作用域链）。因为javaScript允许使用内部函数---即函数定义和函数表达式位于另一个函数的函数体内。而且，这些内部函数可以访问它们所在的外部函数中声明的所有局部变量、参数和声明的其他内部函数。当其中一个这样的内部函数在包含它们的外部函数之外被调用时，使用了包含它们的外部函数作用域内的变量等，就会形成闭包。
    - 为了理解上面的话，我们来看看下面的例子

<!-- more -->

```javaScript
//利用闭包来访问变量 标明为A代码
function foo() {
    var a = 2;
    function bar() {
        console.log(a);
    }
    return bar; //这里返回的是一个函数引用
}

var baz = foo(); //这里仅仅是foo函数返回的一个bar函数引用。
baz(); // 2, 闭包啊

//另一段代码， 标明为B代码
function bar() {
    console.log(a);
}
var baz = bar;
baz(); //出错
```

+ ~~
  - 现在我们来分析下上面的两段代码，代码A 运行到`baz()`时得到的仅仅是一个foo函数返回的函数引用，它没有声明和定义变量a，代码B运行到`baz()`也得到了一个函数引用。这两个函数的定义是完全一样的，也就是其函数的代码是一样了，而且其运行的环境也是一样的，它们都是在全局作用域下运行；代码一样，环境一样，但为什么输出会不一样呢？这也就是闭包的作用了。A代码的baz函数与B代码的baz函数的不同就在于A代码的bar函数是定义在foo对象里面的，而这个对象里面有一个变量a；而bar函数就没有; 这也就是说A代码中的baz函数即使没有在foo的作用域下运行其也可以访问到foo的内部变量，至于为什么会产生闭包，下面会解析；到这里可以回到前面再看看闭包的定义

  - 要区分好利用闭包访问变量和向上搜索作用域链访问变量的区别，请看如下的例子：

```javaScript
//利用闭包来访问变量 标明为A代码
function foo() {
    var a = 2;
    function bar() {
        console.log(a);
    }
    return bar; //这里返回的是一个函数引用
}

var baz = foo(); //这里仅仅是foo函数返回的一个bar函数引用。
baz(); // 2, 闭包啊

//向上搜索作用域链访问变量 标明为B代码
function foo() {
    var a = 2;
    function bar() {
        console.log(a);
    }
    bar(); //运行了代码
}

foo(); //2,这里就是向中搜索作用域链访问变量了
```

+ ~~
  - 那么上面的代码区别在哪里呢？对于代码A来说，利用的是闭包访问到了变量a，对于代码B来说，是由于作用域链，示意图如下
    - ![作用域链](http://7xncgn.com1.z0.glb.clouddn.com/16-3-2/15578701.jpg)
    - 当函数运行bar()时，bar()在其函数作用域下找不到变量`a`，就到其上一层作用域 ，也就是foo函数作用域，找到并输出了变量a; 这与闭包是不同的，在闭包的情况下，运行bar()函数时，并没有函数foo在函数栈中。
    
+ 为什么会产生闭包呢，又是如何产生的，存储在哪里呢？
    - 通过《你不知道的javaScript》一书可知，这是由于javaScript是一种基于词法作用域的语言；只要出现函数在非其被定义的作用域下访问该作用域的变量时就会出现闭包了。
    - javaScript有函数作用域
    - 那么是如何产生的，存放在哪里呢？用javaScript是无法访问到闭包对象的，在这里我们使用chrome来具体看下
    ![](http://7xncgn.com1.z0.glb.clouddn.com/16-3-2/31549008.jpg)
    - 所以我们可以这么看，由于foo作用域里面的bar()函数仍然存在，所以foo作用域其实并没有被销毁，因为bar函数作用域有这一作用域的引用，而这一引用就叫闭包。

+ 那么闭包有什么作用呢？ **本质都是实现数据"封闭"，并在于内存中** 
    - 利用匿名函数来创建闭包，构建命名空间，以减少全局变量的使用,这样做可以不污染全局对象。
    - 结果缓存，因为它不会释放外部函数的引用，从而函数内部的值可以得以保留
    - 封闭，为变量提供getter和setter,而不是直接访问内部变量
    - 模块化
        * 模块化有两个特征
            + 为创建内部作用域而调用了一个包装函数
            + 包装函数的返回值至少包括一个对内部函数的引用，这样就会创建涵盖整个包装函数内部作用域的闭包了
+ 关于利用匿名函数创建闭包，防止污染全局对象的例子(例子来自 [链接](http://www.cnblogs.com/rainman/archive/2009/05/04/1448899.html))
``` 
var oEvent = {};
(function(){
    //其他变量定义 
    var addEvent = function(){ /*代码的实现省略了*/ };
    function removeEvent(){}

    oEvent.addEvent = addEvent;
    oEvent.removeEvent = removeEvent;
})();
```
通过上述的匿名闭包就可以将原本要定义在全局的addEvent函数等变量定义在一个私有的作用域里面了，同时通过oEvent又可以访问到addEvent等属性

+ 关于闭包的模块化，用书中的模块管理器代码来说明，其本质就是将这种模块定义封闭进一个友好的API中
```javaScript
var MyModules = (function Manager() {
    var modules = {};
    function define(name, deps, impl) {
        for (var i=0; i<deps.length; i++) {
            deps[i] = modules[deps[i]]; //通过模块名得到真正的模块对象
    }
        modules[name] = impl.apply( impl, deps );
    }
    function get(name) {
        return modules[name];
    }
    return {
        define: define,
        get: get
    };
})(); //自执行
//定义模块
MyModules.define( "bar", [], function() {
    function hello(who) {
        return "Let me introduce: " + who;
    }
    return {
        hello: hello
    };
} );
MyModules.define( "foo", ["bar"], function(bar) {
    var hungry = "hippo";
    function awesome() {
        console.log( bar.hello( hungry ).toUpperCase() );
}
    return {
        awesome: awesome
    };
} );
var bar = MyModules.get( "bar" );
var foo = MyModules.get( "foo" );
```
+ 对上述的代码进行说明下
    - MyModules.define里面的三个参数分别表示，模块名称，模块依赖表，还有模块的属性创建函数；这里利用闭包，通过Manager来管理所有的模块也就是moduels变量，这个变量可以通过Manager函数的内部函数define和get通过闭包进行访问，从而实现了模块的集中管理

+ 总结下闭包的优点与缺点
    - 闭包的优点：
        * 可以让一个变量常驻内存 (如果用的多了就成了缺点)
        * 避免全局变量的污染
        * 私有化变量

    - 闭包的缺点
        * 因为闭包会携带包含它的函数的作用域，因此会比其他函数占用更多的内存
        * 引起内存泄露(垃圾回收机制中的循环引用)

### 补充
```javaScript
var promise = {
    name: 'kinthon',
    test: function() {
        console.log(this === promise);
        console.log(promise);
    }
}
promise.test();
var t = promise.test;
console.log(t);
t();
```
+ 通过上面代码的运行我们可以得到在对象里面可以通过对象名来得到整个对象的引用,即使运行上下文不是promise也是一样的。
### 参考链接
+ https://segmentfault.com/a/1190000002778015#articleHeader1
+ http://www.cnblogs.com/rainman/archive/2009/05/04/1448899.html

