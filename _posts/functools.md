title: python functools 模块
date: 2015-07-02 18:05:05
categories: python
tags: [python, 标准库]
---
#### functools.total_ordering(cls)
> 给予类丰富的排序方法，使用装饰器简化了操作。如果使用必须在类里面定义一个`__lt__()`,`__le__()`, `__gt__()`, 或`__ge__()`。应该给类添加一个`__eq__()` 方法。

```python
from functools import total_ordering

@total_ordering
class Student(object):
    def __init__(self, name):
        self.name = name

    def __eq__(self, other):
        return self.name.lower() == other.name.lower()

    def __lt__(self, other):
        return self.name.lower() < other.name.lower()

a = Student('dan')
b = Student('mink')

print a > b
print a
print sorted([b, a])
```
打印结果
```python
False
<__main__.Student object at 0x7f16ecb194d0>
[<__main__.Student object at 0x7f16ecb194d0>, <__main__.Student object at 0x7f16ecb195d0>]
```
<!--more-->
#### functools.partial(func,[\*args][\*\*kwargs])
> `partial`主要固定已有函数的部分参数，并返回一个可调用的函数

```python
import functools 

def article(title, content, author='mink'):
    print 'author: ', author
    print '\tarticle title: ', title
    print '\tarticle content: ', content

season = functools.partial(article, 'spring')
season('spring is a good season.')
season('spring is a bad season.', author='mmm')
```
打印结果
```python
author:  mink
    article title:  spring
        article content:  spring is good season.
author:  mmm
    article title:  spring
        article content:  spring is bad season.

```

