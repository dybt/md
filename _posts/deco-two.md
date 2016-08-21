title: python 装饰器(2)
date: 2015-04-10 15:28:10
categories: python
tags: [python, 闭包与装饰器]
---
### 带参数的装饰器
如果decorator需要传入参数, 那就需要在写一个返回decorator的高阶函数. 写出来更复杂.
```python
def login(level):
    def _deco(func):
        def wrapper(*args, **kwargs):
            if level >= 5:
                print '用户 VIP 等级 %d' % int(level-5)
            else:
                print '用户 屌丝 等级 %d' % abs(level-5)
            return func(*args, **kwargs)
        return wrapper
    return _deco

@login(5)
def user(username):
    print 'welcome, %s' % username

# 用户vip 等级0
# welcome, mink
user('mink')       
```

带参数的decorator等于func = 装饰器函数(装饰器参数)(func)
<!--more-->

### 装饰器类
通过类的`__call__`可以想使用函数一样使用类
```python
class A(object):
    def __init__(self, func):
        self.func = func

    def __call__(self):
        return self.func() ** 2

@A
def foo():
    return 10

print foo()     # print 100
```

