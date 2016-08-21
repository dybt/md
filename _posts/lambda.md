title: lambda 匿名函数
date: 2015-04-10 11:59:11
categories: python
tags: [python]
---
python提供函数式编程的方法,匿名函数就是其中之一.

### 匿名函数 lambda
不需要定义函数,像表达式一样使用,不需要函数名(很多时候名字让我很困扰),一些简单的函数简单化, 举个例子
我需要两个整数相加的函数,通常是这么定义的
``` bash
def add(x, y):
    return x + y
```
<!--more-->
很好的完成了我需要的功能, 但是我现在需要一个数字与字符串相加的函数
``` bash 
def addstr(x, y):
    return x + str(y)
```
又一次完成了我的需求,但是 我突然需要两个整数相减,相除的功能这样函数就得 一直写下去, 但是使用lambda 匿名函数可以直接使用
``` bash 
# 相加的实现
f = lambda x, y: x + y

f_str = lambda x, y: x + str(y)
```
简化了操作 让函数更简单,但有个缺点就是 可维护性差, 当需要功能复杂时不建议使用

