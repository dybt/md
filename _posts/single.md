title: python 单例模式
date: 2015-06-10 11:11:21
categories: python
tags: [python, 类]
---
### 单例模式
<hr>
单例模式保证全局只有一个实例
```python
    class Single(object):
        obj = None
        def __new__(cls, *args, **kwargs):
            if Single.obj is None:
                Single.obj = object.__new__(cls, *args, **kwargs)
            return Single.obj

    a = Single()
    b = Single()

    # <__main__.Single object at 0x7fd075f23810>
    # <__main__.Single object at 0x7fd075f23810>
    print a
    print b 

    a.name = 'mink'

    # mink 139942306711568
    # mink 139942306711568
    print a.name, id(a)
    print b.name, id(b)
```
上面的两个实例其实是一样的, 单例模式保证全局只有一个实例. 
<!--more-->

>`Single`通过obj判断该类是否创建了实例，如果没有创建，就创建一个实例并返回，如果创建了实例就返回实例，保证类只有一个实例.

### 共享属性
在看别人文档的时候看到了有意思的东西, 那就是实例的共享属性。原理是创建实例的时候把实例的`__dict__`指向到同一个字典(内存地址), 这样它们就具有相同的属性和方法。

```python
class A(object):
    _store = []
    def __new__(cls, *args, **kwargs):
        obj = object.__new__(cls, *args, **kwargs)
        obj.__dict__ = cls._store
        return obj

class B(A):
    name = 'mink'

a = A()
aa = A()
b = B()

a.name = 'mink1'
print a.__dict__
print aa.__dict__
print b.__dict__
```
输出结果
```python
{'name': 'mink1'}
{'name': 'mink1'}
{'name': 'mink1'}
```
把它们的地址和`__dict__`打印出来

```python
print a.__dict__, A.__dict__, A.__dict__['_store']
print b.__dict__, B.__dict__
print id(a.__dict__), id(b.__dict__), id(A.__dict__['_store'])
```
输出结果

```python
{'name': 'mink1'} {'__module__': '__main__', '_store': {'name': 'mink1'}, '__new__': <staticmethod object at 0x7febe319ebb0>, '__dict__': <attribute '__dict__' of 'A' objects>, '__weakref__': <attribute '__weakref__' of 'A' objects>, '__doc__': None} {'name': 'mink1'}
{'name': 'mink1'} {'name': 'mink', '__module__': '__main__', '__doc__': None}
140651104203112 140651104203112 140651104203112
```
这说明实例的共享属性是通过，创建一个变量`_store`之后把陆续创建出来的实例的`__dict__`属性与`_store`绑定相同的内存地址来完成共享。
