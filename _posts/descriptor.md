title: python 描述器
date: 2016-01-07 15:39:09
categories: python
tags: [python, 内置函数]
---
### 描述器
---

> 在类中定义了`__get__`, `__set__`, `__delete`这些方法的话就被认为是一个描述器(descriptor)。

属性的访问控制是通过对象的字典(__dict__属性)来获取，设置，删除的。例如: `a.x`会先查`a.__dict__['x']`, 如果没有会在类里面找属性。

### 协议
---
`descr.__get__(self, obj, type=None) --> value`
`descr.__get__(self, obj, value) --> None`
`descr.__delete__(self, obj) --> None`

一个对象如果是一个描述器，`被当做对象属性`(很重要)时重写默认的查找行为。

如果一个对象同时定义了`__get__`和`__set__`，它叫data descriptor。仅定义了`__get__`的描述器叫non-data descriptor。

data descriptor和non-data descriptor区别在于: 相对于实例的字典的优先级，如果实例字典有与描述器具同名的属性，如果描述器是data descriptor，优先使用data descriptor。如果是non-data descriptor，优先使用字典中的属性。

<!--more-->

```python
class B(object):

    def __init__(self):
        self.name = 'mink'

    def __get__(self, obj, objtype=None):
        return self.name

class A(object):
    name = B()

a = A()
print a.__dict__    # print {}
print a.name        # print mink
a.name = 'kk'       
print a.__dict__    # print {'name': 'kk'}
print a.name        # print kk
```
这里B是一个non-data descriptor所以当`a.name = 'kk'`的时候，`a.__dict__`里会有name属性, 接下来给它设置`__set__`

```python

def __set__(self, obj, value):
    self.name = value

 ... do something

a = A()
print a.__dict__    # print {}
print a.name        # print mink
a.name = 'kk'       
print a.__dict__    # print {}
print a.name        # print kk
```
因为data descriptor访问属性优先级比实例的字典高，所以`a.__dict__`是空的。

### 参考
1. [PyZh描述器引导](http://pyzh.readthedocs.org/en/latest/Descriptor-HOW-TO-Guide.html)
