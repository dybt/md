title: 豆瓣发布的python高级编程实例
date: 2015-06-29 17:27:55
categories: python
tags: [python,]
---
本文是对豆瓣发布的python高级编程进行了简单汇总.[传送门](http://www.slideshare.net/hongqn/python-9915982?qid=e166e94d-5f2d-458e-8052-75c4375faa4d&v=qf1&b=&from_search=15)

#### 列表的左插入
对列表的`insert`方法和和`collections.deque`的`appendleft`进行对比,下面是测试代码
```python
import time
import collections

# 这里使用装饰器计时
def show_time(func):
    def _deco():
        st = time.clock()
        func()
        et = time.clock()
        delta = et - st
        print 'call %s() used %fs' % (func.__name__, delta)
        return delta
    return _deco

@show_time
def f1():
    lst = []
    for i in xrange(10000):
        lst.insert(0, i)

@show_time
def f2():
    lst = collections.deque()
    for i in xrange(10000):
        lst.appendleft(i)

a = f1()
b = f2()

print a / b
```
<!--more-->

对比会print出来

```python
sorted(lst, reverse=True)[:10]

heapq.nlargest(10, lst)

result: a 与 b的比例是 0.227...
```
#### 装饰器缓存
>装饰器缓存提速

```python
def fin(n):
    if n <= 1:
        return 1
    return fib(n-2) + fib(n-1)

fib(25)

result: 花费时间是0.148837
```
这次用装饰器缓存试一下,可以查看之间的区别
```bash
def cache(func):
    c = {}
    def _deco(n):
        r = c.get(n)
        if r is None:
            r = c[n] = func(n)
        return r
    return _deco

@show_time
@cache
def fib(n):
    if n <= 1:
        return 1
    return fib(n-2) + fib(n-1)

result: 花费时间0.000198
```
#### local变量与global变量
>局部变量比全局变量快

```python
@show_time
def f1():
    for i in xrange(10000):
        r = abs(i)

@show_time
def f2():
    _abs = abs
    for i in xrange(10000):
        r = _abs(i)
    
result: global used 0.000362 
        local  used 0.000297
```
#### 列表解析与循环
> 列表解析快于循环

```python
@show_time
def f1():
    lst = []
    for i in xrange(10000):
        lst.append(i)
        
@show_time
def f2():
    lst = [i for i in xrange(10000)]

result: call f1() 0.000568
        call f2() 0.000192
a/b   : 2.93222222
```
#### 使用while时的判断
> 使用`while`时 `1`比`True`快

```python
@show_time
def f1():
    a = 0 
    while True:
        a += 1
        if a > 10000:
            break

@show_time
def f2():
    a = 0
    while 1:
        a += 1
        if a > 10000:
            break

result: call f1() 0.000655
        call f2() 0.000425
a/b   : 1.5411
```

