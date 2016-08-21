title: python 偏函数
date: 2015-04-10 16:19:52
categories: python
tags: [python]
---
### 偏函数
python 中提供一种用于对函数固定属性的函数(与数学上的偏函数不一样)
```python
# 通常会返回10进制
int('12345')        # print 12345 

# 使用参数 返回 8进制
int('11111', 8)     # print  4681
```
<!--more-->
每次都得添加参数比较麻烦, functools提供了partial的方法
```python
import functools

foo = functools.partial(int, base=8)

foo('11111')        # print 4681
```
通过这种方法生成一个固定参数的新函数.

