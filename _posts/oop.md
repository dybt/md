title: property __slots__ 和 __all__
date: 2015-04-14 15:47:19
categories: python
tags: [python,]
---
python中有很多实用的方法下面的几个都是经常使用的

### @property
**@property** 是与**@staticmethod**与**@classmethod**一样python内置的装饰器.作用是把方法当成属性来调用
```python    
    class Person(object):
        
        @property
        def say_name(self):
            return self.name
            
    me = Person()
    me.name = mink
    me.say_name                     # print 'mink'
```
<!--more-->

m.say_name()变成了m.say_name就可以调用了, 想属性一样
@property会创建出另一个装饰器@<func_name>.setter,func_name是被**@property**装饰的函数名

```python
@property
def say_name(self):
    return self.name

@say_name.setter
def say_name(self, name)
    self.name = name

m.say_name = 'mink2'
m.say_name                      # print 'mink2'
```
这样就可以想属性一样赋值, 添加@property的函数是只读的,而只有设置了**setter**方法的函数才可以**读写**.

### \_\_slots\_\_
python中通过`__slots__`限制类的属性

```python
# 定义一个person类
class Person(object):
    pass

me = Person()
me.name = 'mink'
me.age = 330 

print m.name, m.age             # print 'mink' 330 
# 但是这样就会暴露我的年龄所以把他取消掉age属性, 修改如下

class Person(object):
    __slots__ = ('name', 'sex')

me = Person()
me.name = 'mink'
me.sex = 'male'
me.age = 30
print me.name, m.sex, m.age     
# 'AttributeError: Person has not attribute age.'  
但是有一种情况`__slots__`不生效, 那就是在经典类(旧事类),具体不做描述

# 经典类
class Person:
    pass

# 新式类(钻石类)
class Person(object):
    pass
```
区别在与一个继承是object的积累, 以及搜索机制的改变

### \_\_all\_\_ 
我们经常在导入外部模块的时候使用**from xxx import \* **的方法.但是我不想导入一些内容则使用`__all__` 方法 
```python
    # 这会告诉python 我只导入__all__里面的变量, 这样from xxx import \* 就不会导入其他的了
    __all__ =  ['a', 'b', 'c']
```
我在这里创建两个文件test1和test2来试一下
```python
    # test1
    import os

    __all__ = ['Person',]

    class Person(object):
        pass

    class Animal(object):
        pass 


    # test2

    from test1 import * 
    a = Person()
    a.name = 'mink'

    print a.name                    # print 'mink'
```
person是可以调用的,在添加一下内容
```python
    # test2
    b = Animal()
    b.name = 'lulu'
    
    print b.name                    # raise 'NameError: name Animal is not defined'
```
