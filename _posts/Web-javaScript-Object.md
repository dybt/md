title: 细说javascript的对象
date: 2016-02-28 09:30:59
tags: Web
---

#### **准备**
+ 代码
```javaScript
function Foo(name) {
    this.name = name;
}
var f1 = new Foo("kinthon");
f1.toString();
```
> 在这里我们只讨论关于对象的，不讨论代码执行上下文的阶段
下文说到的构造子就是类似于Java语言中的构造函数

+ javaScript中的this [参考](http://www.ruanyifeng.com/blog/2010/04/using_this_keyword_in_javascript.html), 但是有一个总的原则，那就是this指的是，调用函数的那个对象。
+ 如何查看chrome中内在变量 
![内存变量](http://7xncgn.com1.z0.glb.clouddn.com/16-2-28/95499329.jpg)
<!-- more -->

#### **说明**
+ 当我们的Foo构造子(也就是Foo函数)被解析完后，内存中的状态为
![Person](http://7xncgn.com1.z0.glb.clouddn.com/16-2-28/82674590.jpg)
可以看出，虽然我们只是添加了一个this.name=name;但是解析器背后为我们处理了大量的操作，常见的arguments对象，还有caller，length以及函数的name; 
+ 这里还要说明下`prototype`
    - 对于prototype这个属性，它是一个对象，类似于一个指针，指向了我们的函数原型；原型的情况如下
    ![prototype](http://7xncgn.com1.z0.glb.clouddn.com/16-2-28/63629692.jpg)
    - 也就是说我们原型一开始有的属性就是一个constructor,这个属性指向了我们Foo函数；所以情况如下：
    ![](http://7xncgn.com1.z0.glb.clouddn.com/16-2-28/30371909.jpg)
    此时思考下，如果我们的原型对象是另一个对象的实例或是有另一个对象的属性和方法的话，而且我们Foo实例有办法访问到原型对象的话，会发生什么？就是相当于继承了，因为在javaScript中的搜索属性与方法是沿着实例的原型链去找的。
+ 当我们执行了 `var f1 = new Foo("kinthon")`;之后，内存情况如下
![f1](http://7xncgn.com1.z0.glb.clouddn.com/16-2-28/50053697.jpg)
可以看出f1属性中有我们设置的`age`属性，同时注意到这里也有一个`__proto__`;通过以上的解释我们知道,如果是Foo对象的话(这里不是表示Foo实例，此时Foo对象的类型为函数) ，它可以通过其prototype来访问Foo的原型，但是对于用Foo这个构造子构造出来时的怎么访问Foo的原型呢(不然实现不了继承)?也就是说实例对象怎么访问其构造子的原型，这个就是通过__proto__属性来实现的。这个属性也类似于一个指针，指向了构造子的原型，所以情况如下
![](http://7xncgn.com1.z0.glb.clouddn.com/16-2-28/33384275.jpg)
+ 现在我们解释下为什么在Foo下既有`prototype`，又有`__proto__`，要理解这两个的作用是不一样的，`prototype`是指向自身的原型，而`__proto__`是指向构造子的原型，那么谁会是Foo的构造子？我们知道Foo本身是一个Function,所以其构造子就是Function了，所以Foo中的`__proto__`也就是指向了Function 的原型了；那么为什么需要这两个属性呢？首先呢`prototype`是用于构造子在构造实例时，让实例对象的`__proto__`去指向构造子原型的；`__proto__`是用于找到构造子的函数原型；
+ 到这里我们总结下，在JS中构造子，也可以说是function,都会有一个prototype对象指向其自身的原型，而原型同样会有一个constructor对象指向其函数；而通过构造子实例化后的对象都会有一个__proto__对象指向了其构造子的原型；
+ 综上我们再看一个现象
![](http://7xncgn.com1.z0.glb.clouddn.com/16-2-28/71094152.jpg)
在prototype中有一个__proto__属性，这个属性指向了Object;不难理解，因为prototype也是一个对象，所以其也会有构造子和其构造子的原型，它的构造子就是Object函数对象了[在js中没有显示指定的构造子函数的对象都是以Object为构造子创建的]；所以到这里我们也就不难理解，为什么我们可以在f1中调用toString()这些Object的方法了。
+ 一个总的链图
![](http://7xncgn.com1.z0.glb.clouddn.com/16-2-28/9687611.jpg)
+ 最后来总的说明下一开始的代码运行
    - 首先javaScript引擎解析了Foo对象，从其定义知道是一个Function对象，使用Function的构造子进行构造，因为Function构造子也是对象，所以要生成Function构造子，当然也需要Function构造子的构造子了，也就是Object构造子，根据链图可以知道，Object的构造子是null; 这样递归下来，就构造出了Foo对象；
    - 然后使用Foo对象做为构造子来生成一个f1对象，也就是实例化Foo,并且注意到了构造内部有this.name,而调用它的是f1，所以f1就有了一个name的属性；同时构造子将其`prototytpe`的值赋给了f1的它的`__proto__`属性；
    - f1调用toString(),这里为了测试的方便，可以将Object的toString方法进行修改，如下
```javaScript
Object.prototype.toString = function() {
  console.log('haha');
}
```
    首先f1在自己的上下文找toString()方法， 并没有找到，所以就沿着原型链找，通过`__proto__`来到了Foo.prototype,发现也找不到,沿着Foo.prototype的`__proto__`来到了Object.prototype,找到了Object.prototype，并找到了toString()