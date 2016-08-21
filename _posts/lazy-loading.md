---
title: python lazy-loading
date: 2016-02-19 16:25:08
categories: python
tags: [python]
---
懒加载是一种很有用的东西，他能在程序真正使用的时候把属性加载到内存中。下面是`werkzeug`中的`cache_property`, 通过描述器改变属性的加载状态。

<!--more-->

```python
class cache_property(object):

    def __init__(self, func, name=None, doc=None):
        self.__name__ = name or func.__name__
        self.__module__ = func.__module__
        self.__doc__ = func.__doc__
        print self.__name__, self.__module__, self.__doc__
        self.func = func

    def __set__(self, obj, value):

        # 如果实例对象给属性赋值，就将被装饰的函数名添加到该实例中
        # f.foo = 33 ==> f.__dict__['foo'] = 3
        obj.__dict__[self.__name__] = value

    def __get__(self, obj, type=None):
        if obj is None:
            return self
        # 判断属性是否在实例的__dict__中
        # f.foo  == > f.__dict__.get('foo')
        value = obj.__dict__.get(self.__name__, None)
        if value is None:
            value = self.func(obj)
            obj.__dict__[self.__name__] = value
        return value


class Foo(object):
    
    @cache_property
    def foo(self):
        return 42

if __name__ == '__main__':
    f = foo()
    print f.__dict__
    f.foo = 44
    print f.__dict__
```
返回结果
```python
foo __main__ None
{}
{'foo': 44}
```
