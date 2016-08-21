title: 类方法和静态方法
date: 2015-04-11 17:32:29
categories: python
tags: [python, 类]
---
python中可以经常看到`@classmethod`和`@staticmethod`, 被称为类方法和实例方法.
```python
class Animal(object):
    name = 'lili'
    age = 1

cat = Animal()
print cat.name, cat.age     # print 'lili' 1
```
<!--more-->
创建了一个动物类, 生成了一个cat的实例, 打印cat的名字和年龄, 可以看出返回的是Animal这个类的属性, 也就是实例访问了类的属性
```python
# 显示内容是一样的
print cat.name, cat.age
print Animal.name, Animal.age
```
给Animal类添加一个方法(函数)
```python
class Animal(object):
    name = 'lili'
    age = 1

    def edit(self, name, age):
        self.name = name
        self.age = age

cat = Animal()
cat.edit('rol', 2)

print cat.name, cat.age         # print 'rol' 2
print Animal.name, Animal.age   # print 'lili' 1
```
也就是说这个默认添加的方法是一个实例的方法, 实例方法修改了实例的属性,而类的属性不改变
```python
# 我们修改一下这个函数
def edit(self, name, age):
    name = name
    self.age = age

cat = Animal()
cat.edit('rol', 2)

print cat.name, cat.age         # pirnt 'rol' 2
print Animal.name, cat.age      # print 'lili' 1
```
说明实例方法不能修改类的属性, 但我想修改类的属性怎么办
```python
# 再一次修改edit
@classmethod
def edit(cls, name, age):
    cls.name = name
    cls.age = age

cat = Animal()
cat.edit('rol', 2)

print cat.name, cat.age         # print 'rol' 2
print Animal.name, Animal.age   # print 'rol' 2
```
这里需要注意的是edit函数的第一个参数有self变为cls, python中建议大家在类的方法中使用cls而实例方法的参数为self, 而且这里说明了实例可以使用类的方法(函数)
那么我在给这个类添加**__init__**方法来初始化属性
```python
class Animal(object):
    name = 'lili'
    age = 1

    def __init__(self, name, age):
        self.name = name
        self.age = age
    ...

cat = Animal('kuku', 4)
cat.edit('rol', 2)

print cat.name, cat.age         # print 'kuku' 4
print Animal.name, Animal.age   # print 'rol' 2
```
添加\_\_init\_\_以后, cat不再使用类的属性,而修改了edit方法也没有改变cat实例的属性.
```python
# 添加staticmethod
@staticmethod
def say_name(name=None):
    if not name:
        name = self.name
    print 'my name is %s.' % name

cat = Animal('kaka', 3)
cat.say_name()                
# 运行的话会报 NameError: global name 'self' is not defined    
# 那是不是没给他添加self字段, 所以没找到
def say_name(self, name=None):
    ...

cat.say_name()
# TypeError: say_name() takes at least 1 argument(0 given), 显示缺少参数
```
这说明staticmethod 不能使用实例的属性和方法, 当然也使用不了类. 那么反过来呢
```python
# 我们修改一下代码
# 先创建一个实例的方法, 他使用类的staticmethod

@staticmethod
def say_name(name):
    print 'my name is %s.' % name

def say(self):
    self.say_name(self.name)
    
@classmethod
def _say(cls):
    cls.say_name(cls.name)

cat = Animal('kaka', 3)
cat.say()
cat._say()
```
可以通过类方法和实例方法访问staticmethod.
总结一下:
静态方法(staticmethod)
* 静态方法不能使用实例的属性和方法
* 静态方法不能使用类的属性和方法
* 静态方法可以通过类或实例调用
* 静态方法等于作用域在类中的全局函数

类方法(classmethod)
* 类方法可以使用类的属性和方法
* 类的方法可以使用静态方法
* 类的方法可以通过类或实例调用
