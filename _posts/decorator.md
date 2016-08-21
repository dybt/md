title: python 装饰器(1)
date: 2015-04-10 13:46:58
categories: python
tags: [python, 闭包与装饰器]
---
在python中常看到在定义函数是使用**@func**. 这就是装饰器, 装饰器是把一个函数作为参数的函数,常常用于扩展已有函数,即不改变当前函数状态下增加功能.
``` python
def run():
    print "I'm run."
```
<!--more-->
我有这么一个函数, 我想知道这个函数什么时候开始什么时候结束. 我应该这么写
```python
def run():
    print time.ctime()
    print "I'm run."
    print time.ctime()
```
但是如果不允许修改函数的话就需要装饰器了
```python
def count(func):
    def wrapper():
        print time.ctime()
        ret = func()
        print time.ctime()
        return ret
    return wrapper

@count
def run():
    print "I'm run."
```

[**转载**]此外把[廖雪峰的官方网站][1]关于装饰器的例子copy下来, 这个网站真的很给力讲的浅显易懂.
[1]: http://www.liaoxuefeng.com "廖雪峰的官方网站"
```python
def now():
    print '2015-4-10'
f = now
f()                 # print '2015-4-10'
```

函数有一个\_\_name\_\_ 对象 可通过 dir(func) func为定义的函数名
```python
now.__name__        # print 'now'
f.__name__          # print 'now'

print f             # print '<function now at 0x000000000213A908>'
print now           # print '<function now at 0x000000000213A908>'
```
我们通过装饰器打印log日志
```python
def log(func):
    def wrapper(*args, **kwargs):
        print "call %s()" % func.__name__
        return func(*args, **kwargs)
    return wrapper

@log
def now():
    print '2015-4-10'

now()               # print 'call now()'
```
其实装饰器修饰函数相当于, now = log(now) 也就是装饰器函数把被修饰的函数当参数后赋给同名的变量
### functools.wraps 函数
当我们使用了装饰器后now的\_\_name\_\_值发生了改变
```python
# 没有使用前
now.__name__        # print 'now'
# 使用后
now.__name__        # print 'wrapper'
```
当我们使用装饰器前,now.\_\_name\_\_使用的是当前now函数,但使用后 now这个函数其实是 log(now) 也就是log函数的返回值也就是被包裹的wrapper. 解决方法是functools.wraps函数.
```python
# 装饰闭包, 使用前得调用 import functools
def log(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        ...
```
