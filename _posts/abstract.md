title: python 抽象类
date: 2015-07-01 11:06:28
categories: python
tags: [python, 类]
---
像java一样python也可以定义一个抽象类

#### 如何使用抽象类
```python
import abc 

class A(object):
    __metaclass__ = abc.ABCMeta

    @abc.abstractmethod
    def load(self, input):
        return 

    @abc.abstractmethod
    def save(self, output, data):
        return
```
通过`ABCMeta元类`来创建一个抽象类, 使用`abstractmethod装饰器`来表明抽象方法
<!--more-->
#### 注册具体类
```python
class B(object):
    
    def load(self, input):
        return input.read()

    def save(self, output, data):
        return output.write(data)

A.register(B)

if __name__ == '__main__':
    print issubclass(B, A)      # print True
    print isinstance(B(), A)    # print True
```
从抽象类注册一个具体的类
#### 子类化实现
```python
class C(A):

    def load(self, input):
        return input.read()

    def save(self, output, data):
        return output.write(data)
        
if __name__ == '__main__':
    print issubclass(C, A)      # print True
    print isinstance(C(), A)    # print True
```
可以使用继承抽象类的方法来实现具体类这样可以避免使用`register`. 但是副作用是可以通过基类找出所有的具体类
```python
for sc in A.__subclasses__():
    print sc.__name__

# print C
```
如果使用继承的方式会找出所有的具体类，如果使用`register`的方式则不会被找出

#### 使用\_\_subclasshook\_\_
使用`__subclasshook__`后只要具体类定义了与抽象类相同的方法就认为是他的子类
```python
import abc

class A(object):
    __metaclass__ = abc.ABCMeta

    @abc.abstractmethod
    def say(self):
        return 'say yeah'

    @classmethod
    def __subclasshook__(cls, C):
        if cls is A:
            if any("say" in B.__dict__ for B in C.__mro__):
                return True
        return NotTmplementd

class B(object):
    def say(self):
        return 'hello'

print issubclass(B, A)      # True
print isinstance(B(), A)    # True
print B.__dict__            # {'say': <function say at 0x7f...>, ...}
print A.__subclasshook__(B) # True
```
#### 不完整的实现
```python
class D(A):
    def save(self, output, data):
        return output.write(data)

if __name__ == '__main__':
    print issubclass(D, A)      # print True
    print isinstance(D(), A)    # raise TypeError
```
如果构建不完整的具体类会抛出D不能实例化抽象类和抽象方法

#### 具体类中使用抽象基类
```python
import abc 
from cStringIO import StringIO

class A(object):
    __metaclass__ = abc.ABCMeta

    @abc.abstractmethod
    def retrieve_values(self, input):
        pirnt 'base class reading data'
        return input.read()


class B(A):

    def retrieve_values(self, input):
        base_data = super(B, self).retrieve_values(input)
        print 'subclass sorting data'
        response = sorted(base_data.splitlines())
        return response

input = StringIO("""line one
line two
line three
""")

reader = B()
print reader.retrieve_values(input)
```
打印结果
```python
base class reading data
subclass sorting data
['line one', 'line two', 'line three']
```
可以使用`super`来重用抽象基类中的罗辑, 但会迫使子类提供覆盖方法.

#### 抽象属性
```python
import abc

class A(object):
    __metaclass__ = abc.ABCMeta

    @abc.abstractproperty
    def value(self):
        return 'should never get here.'

class B(A):
    
    @property
    def value(self):
        return 'concrete property.'

try:
    a = A()
    print 'A.value', a.value
except Exception, err:
    print 'Error: ', str(err)

b = B()
print 'B.value', b.value 
```
打印结果,`A`不能被实例化，因为只有一个抽象的`property getter method`.
```python
Error: ...
print concrete property
```
定义抽象的读写属性
```python
import abc

class A(object):
    __metaclass__ = abc.ABCMeta

    def value_getter(self):
        return 'Should never see this.'

    def value_setter(self, value):
        return 

    value = abc.abstractproperty(value_getter, value_setter)

class B(A):
    
    @abc.abstractproperty
    def value(self):
        return 'read-only'

class C(A):
    _value = 'default value'

    def value_getter(self):
        return self._value

    def value_setter(self, value):
        self._value = value

    value = property(value_getter, value_setter)

try:
    a = A()
    print a.value
except Exception, err:
    print str(err)

try:
    b = B()
    print b.value
except Exception, err:
    print str(err)

c = C()
print c.value

c.value = 'hello'
print c.value
```
打印结果, 定义具体类的`property`时必须与抽象的`abstract property`相同。如果只覆盖其中一个将不会工作.
```python
error: ...
error: ...
print 'default value'
print 'hello'
```
使用装饰器语法来实现读写的抽象属性, 读和写的方法应该相同.
```python
import abc

class A(object):
    __metaclass__ = abc.ABCMeta

    @abc.abstractproperty
    def value(self):
        return 'should never see this.'

    @value.setter
    def value(self, _value):
        return 

class B(A):
    _value = 'default'

    @property
    def value(self):
        return self._value

    @value.setter
    def value(self, _value):
        self._value = _value

b = B()
print b.value       # print 'default'

b.value = 'hello'
print b.value       # print 'hello'
```
#### 参考文献
* [pymotw](http://pymotw.com/2/abc/)
* [Why use Abstract Base Classes in Python?](http://stackoverflow.com/questions/3570796/why-use-abstract-base-classes-in-python)
