title: python slice对象与__getitem__
date: 2016-01-05 16:24:22
categories: python
tags: [python, 内置函数]
---
想要使类的实例像列表一样使用下标, 可以设置`__getitem__`方法。比如:

```python
class _List(object):

    def __getitem__(self, key):
        print key

l = _List()
l[3]    # print 3
```
但是如果想要使用切片操作的

```python
l[1:4]  # print slice(1, 4, None)
```
会创建一个slice对象用于切片,　可以通过`help(slice)`查看具体操作。

```python
a = slice(1, 4, None)

range(5)[a]  # print [1, 2, 3]
```
<!--more--> 

更加丰富的操作
```python
class _List(object):
    
    def __init__(self, _list):
        self._list = _list

    def __getitem__(self, key):
        if isinstance(key, int):
            return self._list[key]
        elif isinstance(key, slice):
            reutrn self.__class__(self._list[key])

if __name__ == '__main__':
    c = _List(range(10))
    b = c[1:5]
    print b[3]  # print 4
```
如果`key`是一个整形的话就返回列表元素，如果是一个slice对象的话，就创建一个实例并返回。

