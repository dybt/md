title: python 类的继承方法
date: 2015-08-31 14:41:36
categories: python
tags: [python, 类]
---
python创建一个类很简单只需要定义它就可以了.
```python
class Cat:
    pass
```
就像这样就可以了，通过创建子类我们可以继承他的父类(超类)的方法。这里重新写一下cat
```python
class Cat:
    name = 'cat'


class A(Cat):
    pass

print A.name    # cat
```
<!--more-->
### 经典类
我们也可以这样，让Ａ多继承。
```python
class Cat:
    name = 'cat'


class Dog:
    name = 'dog'


class A(Cat, Dog):
    pass

print A.name    # cat 
```
如果Cat类没有name属性呢?
```python 
class Cat:
    pass

    ...
print A.name    # dog
```
A就会在其他的父类中找name这个属性。如果继承的两个父类都是继承自Animal类而Animal类也有name属性呢?
```python
class Animal:
    name = 'animal'


class Cat(Animal):
    pass


class Dog(Animal):
    name = 'dog'


class A(Cat, Dog):
    pass

print A.name    # animal
```
这样A就不会在Dog类中找而是会在Animal上找到name, 这种类叫经典类。类的解析顺序是一种从左到右深度优先的搜索。也就是A--> Cat--> Animal --> Dog。
### 新式类
python还有一种创建类的方式，就是使用新式类(建议使用), 都继承自object这个基类, 新式类的搜索规则是从左到右逐级查询。也就是A--> Cat --> Dog --> Animal。
```python
class Cat(object):
    pass
```
