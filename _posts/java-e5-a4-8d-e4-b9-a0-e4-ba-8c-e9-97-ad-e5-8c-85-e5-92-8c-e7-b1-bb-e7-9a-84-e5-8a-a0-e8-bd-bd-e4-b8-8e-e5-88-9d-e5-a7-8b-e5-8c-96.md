---
title: java复习(二) 闭包和类的加载与初始化
id: 76
categories:
  - Java复习
date: 2015-05-31 07:23:32
tags:
    - Java
    - 闭包
    - 类的加载与初始化
---

<span style="font-size: 20px;">参考链接:[http://blog.sina.com.cn/s/blog_8d2da7430100xjk5.html](http://blog.sina.com.cn/s/blog_8d2da7430100xjk5.html) (闭包)</span>

<span style="font-size: 20px;">参考链接:[http://www.importnew.com/6579.html](http://www.importnew.com/6579.html)(类的加载和初始化)</span>

<span style="font-size: 20px;">一个类加载的时候只是会初始化它的static成员变量和static域(从上往下依次加载),而类的初始化则必须在加载的基础上依次初始化非static变量和非static域(从上往下依次加载),然后调用类的构造器,进行初始化.若一个类中包含父类,则必须先初始化父类(当一个类要加载时也是同样的),这样递归到没有父类的类,然后父类到子类依次执行初始化顺序,首先从父类到子类依次加载类,然后从父类到子类依次初始化静态域和静态常量,从父类到子类依次 初始化非静态域和非静态常量,和构造器(先初始非静态的变量,然后初始化构造器)</span>

&nbsp;

&nbsp;

<span style="font-size: 20px;">闭包的概念:链接中已经挺详细的,这也是内部类的一个用途</span>