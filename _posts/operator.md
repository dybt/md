title: python operator 模块
date: 2015-07-10 10:35:23
categories: python
tags: [python, 标准库]
---
函数式编程使用迭代器需要创建功能简单的表达式。有时可以使用lambda函数创建， 但有些操作不需要自定义函数。operator模块定义了内置的运算操作用于比较序列和字典。
## 逻辑操作
operator的比较操作
```python
from operator import *

a = [1, 2, 3]
b = a
print 'a =', a
print 'b =', b
print 

print 'not_(a)      :', not_(a)
print 'truth(a)     :', truth(a)
print 'is_(a, b)    :', is_(a, b)
print 'is_not(a, b) :', is_not(a, b)
```
打印结果
```python
a = [1, 2, 3]
b = [1, 2, 3]
not_(a)     : False
truth(a)    : True
is_(a, b)   : True
is_not(a, b): False
```
可以通过结果知道，operator的一些操作函数与原本的运算是相同的。
<!--more-->
## 比较操作
operator提供丰富的比较操作。
```python
a = 3
b = 5
print 'a =', a
print 'b =', b
print 

for func in (lt, le, eq, ne, ge, gt):
    print '{0}(a, b):'.format(func.__name__), func(a, b)
```
打印结果
```python
a = 3
b = 5

lt(a, b): True
le(a, b): True
eq(a, b): False
ne(a, b): True
ge(a, b): False
gt(a, b): False
```
## 运算操作
```python
a, b, c, d = -1, 2, -3, 4

print 'a =', a
print 'b =', b
print 'c =', c
print 'd =', d
 
print '\nPositive/Negative:'
print 'abs(a):', abs(a)
print 'neg(a):', neg(a)
print 'neg(b):', neg(b)
print 'pos(a):', pos(a)
print 'pos(b):', pos(b)
```
打印结果
```python
a = -1
b = 2
c = -3
d = 4

Positive/Negative:
abs(a): 1
neg(a): 1
neg(b): -2
pos(a): -1
pos(b): 2
```
abs返回值得绝对值，neg返回(-obj), pos返回(+obj)。
```python
a = -2
b = 5.0

print 'a =', a
print 'b =', b
 
print '\nArithmetic'
print 'add(a, b)        :', add(a, b)
print 'div(a, b)        :', div(a, b)
print 'floordiv(a, b)   :', floordiv(a, b)
print 'mod(a, b)        :', mod(a, b)
print 'mul(a, b)        :', mul(a, b)
print 'pow(a, b)        :', pow(a, b)
print 'sub(a, b)        :', sub(a, b)
print 'truediv(a, b)    :', truediv(a, b)
```
打印结果
```python
a = -2
b = 5.0

Arithmetic
add(a, b)        : 3.0
div(a, b)        : -0.4
floordiv(a, b)   : -1.0
mod(a, b)        : 3.0  # 查看负数取模
mul(a, b)        : -10.0
pow(a, b)        : -32.0
sub(a, b)        : -7.0
truediv(a, b)    : -0.4
```
mod表示取模， mul 表示相乘，pow是次方, sub表示相减 
```python
a = 2
b = 6

print 'a =', a
print 'b =', b

print '\nBitwise:'
print 'and_(a, b)   :', and_(a, b)
print 'invert(a)    :', invert(a)
print 'lshift(a, b) :', lshift(a, b)
print 'or_(a, b)    :', or_(a, b)
print 'rshift(a, b) :', rshift(a, b)
print 'xor(a, b)    :', xor(a, b)
```
打印结果
```python
a = 2
b = 6

Bitwise:
and_(a, b)   : 2
invert(a)    : -3
lshift(a, b) : 128
or_(a, b)    : 6
rshift(a, b) : 0
xor(a, b)    : 4
```
and_ 表示按位与, invert 表示取反操作, lshift表示左位移, or_表示按位或, rshift表示右位移,xor表示按位异或。
## 序列操作
```python
a = [1, 1, 2, 3]
b = ['a', 'b', 'c']

print 'a =', a
print 'b =', b

print '\nConstructive:'
print 'contact(a, b)    :', concat(a, b)
print 'repeat(a, 3)     :', repeat(a, 3)

print '\nSearching:'
print 'contains(a, 1)   :', contains(a, 1)
print 'countof(a, 1)    :', countOf(a, 1)
print 'indexOf(a, 5)    :', indexOf(a, 1)

print '\nAccessItems:'
print 'getitem(b, 1)    :', getitem(b, 1)
print 'getslice(a, 1, 3):', getslice(a, 1, 3)
print 'setitem(b, 1, "d"):', setitem(b, 1, "d"), ' after b = ', b
print 'setslice(a, 1, 3, [4, 5]):', setslice(a, 1, 3, [4, 5]), ' after a =', a

print '\nDestructive:'
print 'delitem(b, 1)    :', delitem(b, 1), ' after b =', b
print 'delslice(a, 1, 3):', delslice(a, 1, 3), ' after a = ', a
```
## in-place 操作
in-place操作， x += y 等同于 x = iadd(x, y), 如果复制给其他变量比如z = iadd(x, y)等同与z = x; z += y。
```python
a = 3
b = 4
c = [1, 2]
d = ['a', 'b']

print 'a =', a
print 'b =', b
print 'c =', c
print 'd =', d
print

a = iadd(a, b)
print 'a = iadd(a, b) =>', a
print

c = iconcat(c, d)
print 'c = iconcat(c, d) =>', c
```
## 获取属性或元素
```python
class A(object):
    def __init__(self, arg):
        super(A, self).__init__()
        self.arg = arg
        
    def __repr__(self):
        return 'A({0})'.format(self.arg)
    
l = [A(i) for i in xrange(5)]
print l
g = attrgetter('arg')
vals = [g(i) for i in l]
print vals

# attrgetter 等同于 lambda x, n = 'attrname': getattr(x, n)
```
打印结果
```python
[A(0), A(1), A(2), A(3), A(4)]
[0, 1, 2, 3, 4]
```
**itemgetter**
```python
itemgetter(1)('abcd')   # print b

itemgetter(1, 3 )('abcdef') # print ('b', 'd')
```
