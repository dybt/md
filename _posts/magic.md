title: python 魔术方法
date: 2015-06-30 15:48:31
categories: python
tags: [python, 内置函数]
---
`python`提供了很多魔术方法，我们将在这里介绍一下具体的使用方法及用途

### 构建和初始化
<hr>
会介绍python构建和初始化
#### \_\_new\_\_
> `__new__`是一个构造器，发生在`__init__`之前，主要定义一些不可便的对象，如`单例模式`, 当程序创建一个实例时会调用此方法。

```python
# 创建一个类的实例，这个时候会使用__new__方法
a = A()
```

#### \_\_init\_\_
> `__init__`也是一个构造器， 主要用于类的初始化, 当创建一个实例，也就是`__new__`运行以后，紧接着会初始化实例。

```python
class A(object):
    # __init__ 的第一个参数常常用作self来表示(这是一种约定，类用cls)    
    def __init__(self, name):
        self.name = name

# a 是参数self ,'mink'是参数 name
a = A('mink')
```
<!--more-->

#### \_\_del\_\_
> 析构器。在python进行垃圾回收时调用, 常常用于关闭文件等等

```python
class A(object):
    def __del__(self):
        print 'deleted'

a = A()

# print deleted
del a

```
对象回收的时候会调用这个方法, 但是`__del__`不会实现`del a`的操作

```python
a = A()

# 会显示很多属性方法， 其中有 __del__ 那么我调用这个方法会不会删除这个实例呢?
dir(a)

# print deleted 调用了 这个方法打印出了 deleted 那么这个对象还在不在
a.__del__()

# <__main__.A at 0x7...> 还在表示 析构器不会触发 del a 等操作
print a
```
### 可调用对象
<hr>
> 添加__call__魔术方法即可

#### \_\_call\_\_
> 使对象可以被调用

```python
class A(object):
    def __init__(self, num):
        self.num = num

    def __call__(self, num1, num2):
        self.num = num1 + num2

a = A(3)

# print 3
print a.num 

a(1, 4)
# print 5
print a.num
```
### 控制属性使用
<hr>
#### \_\_getattr\_\_
> 属性访问，如果访问的属性不存在会调用此方法, 如果存在则不会调用。

```python
class A(object):
    def __init__(self):
        self.name = 'mink'

    def __getattr__(self, key):
        print 'Invoke __getattr__ method'

a = A()

# print mink
print a.name 

# print 'Invoke __getattr__ method'
a.age

a.age = 13

# print 13
print a.age
```
如果设置了此方法，访问属性不存在时也不会报错, 而且可以显示的添加属性。

#### \_\_getattribute\_\_
> 与`__getattr__`相似都是访问属性，并且优先级高于`__getattr__`，但是这个魔术方法会限制你显示的为实例添加属性, 而且重写此方法时，得注意无限调用自身的Bug. 

其实属性的访问是通过调用`__getattribute__`来访问`__dict__`里的属性 .下面介绍`__getattribute__`.


```python
class A(object):

    def __init__(self):
        self.name = 'mink'

    def __getattribute__(self, key):
        print 'Invoke __getattribute__ method'

a = A()

# print 'Invoke __getattribute__ method'
print a.name 

# print 'Invoke __getattribute__ method'
a.age = 3
print a.age 
```
所有属性的调用都会通过`__getattribute__`，而且不能显示添加属性。而且连已有的属性都不能访问,那么通过字典属性访问一下看看。

```python
def __getattribute__(self, key):
    print 'Invoke __getattribute__ method'
    return self.__dict__[key]

a = A()

# print RuntimeError: maxinum recursion
print a.name
```
抛出异常，这是因为`self.__dict__[key]`也会通过`object.__getattribute__(key)`来查找，这样就又调出了实例的`__getattribute__`方法产生了递归。

可以通过`object.__getattribute__(实例或类, 属性)`来访问, 但是如果没有属性就会抛出异常。

```python
class A(object):

    def __init__(self):
        self.name = 'mink'

    def __getattribute__(self, key):
        print 'Invoke __getattribute__ method'
        return object.__getattribute__(self, key)

a = A()

# print 'mink'
print a.name

# print AttirubeError
print a.age
```

#### \_\_setattr\_\_
> 与`__getattr__` 类似,用于设置属性

```bash
class A(object):
    def __init__(self, name):
        self.name = name

    def __setattr__(self, name, value):
        self.__dict__[name] = value

a = A('mink')

# print 'mink'
print a.name
```

### 下标和键
---
实例可以使用下标和键值对

#### \_\_getitem\_\_
> 在类中实现`__getitem__`接可以获取下标和键值对

```python
class A(object):
    
    def __init__(self):
        self._list = [0, 1, 2, 3]
        self._dict = {'name':'mink', 'age': 25}

    def __getitem__(self, key):
        if isinstance(key, int):
            return self._list[key]
        elif isinstance(key, str):
            return self._list[key]

a = A()
a[0]      # print 0
a['name'] # pirnt mink
```

#### \_\_setitem\_\_
> 与`__getitem__`相似，`__setitem__`用于设置

```python
class A(object):

    def __init__(self):
        self._list = [0, 1, 2, 3]
        self._dict = {'name':'mink', 'age': 25}

    def __getitem__(self, key):
        if isinstance(key, int):
            return self._list[key]
        elif isinstance(key, str):
            return self._dict[key]

    def __setitem__(self, key, value):
        if isinstance(key, int):
            self._list[key] = value
        elif isinstance(key, str):
            self._dict[key] = value
```
