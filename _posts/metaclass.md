title: python 元类
date: 2015-06-10 11:28:32
categories: python
tags: [python, 类,]
---
**动态类型语言**: 是指程序运行时可以改变其结构，新的函数被引进，已有的函数被删除等数据结构上的变化。比如`python`。

### type 
`type`函数可以查看一个变量的类型, 比如:
```python
# <type 'int'>
# <type 'str'>
type(1)         
type('mink')    
```
<!--more-->
`type`函数还可以创建一个新的对象
`type`接受三个参数，`name`, `bases`, `dict` 第一个接受类名，第二个参数接受父类(元组形式),第三个参数接受属性和方法(字典形式)
```python
X = type('X', (object,), dict(a=1))
# 等于
class X(object):
    a = 1
```
下面是接受函数的方法
```python
def say(self):
    print 'hello'

X = type('X', (object,), dict(say=say))
x = X()

# pirnt hello
x.say()
```

### 元类
<hr>
我们都知道通过类可以创建处实例对象，而元类就是创建出类对象的类。`type`可以创建出类对象也就是说`type`就是一个元类。

#### __metaclass__ 属性
如果想使用元类创建类对象就需要对该对象添加一个`__metaclass__`属性。当然你首先得有一个元类
```python
class PrivateMetaclass(type):
    def __new__(cls, name, parents, attrs):
        attrs = dict(('__%s' % k, v) for k, v in attrs.itmes())
        return super(PrivateMetaclass, cls).__new__(cls, name, parents, attrs)

class A(object):
    __metaclass__ = PrivateMetaclass
    a = 1
    b = 2

a = A()
# raise AttributeError
print a.a, a.b 

# print 1, 2
print a.__a, a.__b
```
这样你就可以通过元类来修改类的一些特性，上面的就是修改变量为私有变量.
