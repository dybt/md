---
title: Java复习(一) 内部类
id: 71
categories:
  - Java复习
date: 2015-05-30 15:24:37
tags:
    - Java
    - 内部类
---

## <span style="font-size: 22px;">原文链接:[http://www.open-open.com/lib/view/open1422791124330.html](http://www.open-open.com/lib/view/open1422791124330.html)</span>

<span style="font-size: 22px;">看了一下内部类的博客,博客上讲的十分的棒,很清楚,但是有些东西还是需要自己来总结一下,巩固基础.</span>

<span style="font-size: 22px;">首先内部类的定义,简单的根据字面意思就是定义在内部中的类,叫做内部类</span>

<span style="font-size: 22px;">内部类又可以分为:</span>

<span style="font-size: 22px;">静态内部类(static 类型的class)</span>

<span style="font-size: 22px;">成员内部类(定义在一个class内部中的class但是不能用static修饰)</span>

<span style="font-size: 22px;">局部内部类(定义在方法中的class)像局部变量一样，不能被public, protected, private和static修饰。</span>

<span style="font-size: 22px;">匿名内部类(未命名的class)</span>

&nbsp;

<span style="font-size: 22px;">内部类的性质:</span>

<span style="font-size: 22px;">首先,成员内部中的属性和方法不能用static修饰! 但可以有static final 的数据成员.成员内部类可以访问外部类的属性和方法,而外部类却无法直接访问内部类中的属性和方法,要想访问内部类中属性和方法需要实例化一个成员内部类对象,然后访问.相同级别的成员内部类之间可以相互访问(包括private属性修饰的,这就意味着private失效).在外部类的static方法中无法直接访问内成员内部类.,可以用于闭包.</span>

&nbsp;

<span style="font-size: 22px;">静态内部类只能访问外部类的static修饰的属性和方法,，这是由Java语法中"静态方法不能直接访问非静态成员"所限定,外部类访问静态内部类的时候可以直接new一个静态内部类对象,也可以通过.外部类名.new 内部类() 来访问,其它类中无法直接访问静态内部类.</span>

&nbsp;

<span style="font-size: 22px;">局部内部类:(此时没有过多了解,先不谈)</span>

&nbsp;

<span style="font-size: 22px;">匿名内部类:(先不谈);**匿名内部类要访问的外部类局部变量必须是final修饰的**</span>