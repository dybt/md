title: python itertools 模块
date: 2015-05-29 18:23:41
categories: python
tags: [python, 标准库,]
---
`python`的标准库有一个非常实用的迭代模块`itertools`,提供了各种循环操作.

#### itertools.chain(\*iterables)
`itertools.chain`会把迭代器连接起来, 依次迭代出来.
```python
from itertools import chain

ch = chain('123', 'ddf', '3id')
for i in ch:
    print i,    # print 1 2 3 d d f 3 i d 
```
`chain.from_iterable(iterable)` 接受一个可迭代的结构, 并迭代出来
```python
from itertools import chain
ch = chain.from_iterable(['123', 'ddf'])
for i in ch:
    print i,   # print 1 2 3 d d f

等于
def from_iterable(iterables):
    # chain.from_iterable(['ABC', 'DEF'])  ---> A B C D E F
    for it in iterables:
        for element in it:
            yield element
```
<!--more-->
#### itertools.combinations(iterable, r)
排列组合,combinations('ABCD', 2) --> AB AC AD BC BD CD , 相当于 一下函数
```python
from itertools import combinations

a = combinations('123', 2)
for i in a:
    print i,    # print ('1', '2'), ('1', '3'), ('2', '3')
```
#### itertools.combinations_with_replacement(iterable, r)
排列组合, combinations_with_replacement('ABC', 2) ---> AA AB AC BB BC CC
```python
from itertools import combinations_with_replacement

a = combinations_with_replacement('123', 2)
for i in a:
    print i,    # print ('1', '1'), ('1', '2'), ('1', '3'), ('2', '2'), ('2', '3'), ('3', '3')
```
#### itertools.compress(data, selectors)
根据`selectors`过滤参数`data`, 如果selectors是`True`则返回
```python
from itertools import compress
a = compress('1234', [False, True, 1, 0])
for i in a:
    print i,    # print 2 3
```
#### itertools.count(start=0, step=1)
无穷循环, 从`start`开始每次增加`step`数
```python
from itertools import count

a = count(1, 3)
for i in a:
    if i > 30:
        break
    print i,    # pirnt 1 4 7 10 13 16 19 22 25 28 
```
#### itertools.cycle(iterable)
无穷循环, 循环`iterable`的元素 cycle('ABCD') --> A B C D A B C D ...
```python
from itertools import cycle

count = 0
a = cycle('1234')
for i in a:
    count += 1
    if count > 15:
        break
    print i,    # print 1 2 3 4 1 2 3 4 1 2 3 4 1 2 3
```
#### itertools.dropwhile(predicate, iterable)
如果符合断言, 一直循环。不符合，则跳出循环并返回元素. dropwhile(lambda x: x< 5, [1, 4, 6, 4, 1])
```python
from itertools import dropwhile 

a = dropwhile(lambda x: x< 5, [1, 4, 6, 4, 1])

for i in a:
    print i, 6, 4, 1
```
#### itertools.groupby(iterable[, key])
根据`key` 这个函数返回一个键和一个生成器,会找出所有相邻的而不是相同的。 
```python
from itertools import groupby
a = [1, 3, 4, 5, 6, 7, 8]
for k, g in groupby(a, lambda: x: x<5):
    print k,
    for i in g:
        print i,
    print '\n'

#  print True 1 3 4
#  print False 5 6 7 8 
```

```python
a = 'abcccddc'
for key, value in groupby(a):
    print key, list(value)

# 'a', ['a']
# 'b', ['b']
# 'c', ['c','c','c']
# 'd', ['d', 'd']
# 'c', ['c']
```

#### itertools.imap(function, \*iterables)
imap(pow, (2, 3, 10), (5, 2, 3)) ---> 32, 9, 1000
```python
from itertools import imap

a = imap(pow, (2, 3, 10), (5, 2, 3))
for i in a:
    print i, 32, 9, 1000
```
#### itertools.islice(iterable, stop)
itertools.islice(iterable, start, stop,[,step])
```python
from itertools import islice 

text = 'ABCDEFG'

a = islice(text, 2)
for i in a:
    print i, # print A B

a = islice(text, 2, 4)
for i in a:
    print i, # print C D

a = islice(text, 2, None)
for i in a:
    print i, # print C D E F G

a = islice(text, 0, None, 2)
for i in a:
    print i, # pirnt A C E G
```
#### itertools.permutations(iterable[,r])
排列组合
```python
from itertools import permutations

a = permutations('AB', 2)
for i in a:
    print i, # print ('A', 'B') ('B', 'A')

a = permutations('123')
for i in a:
    print i, # print tuple 123, 132, 213, 231, 312, 321
```
#### itertools.product(\*iterables[, repeat])
```python
from itertools import product

a = product(range(2), repeat=2)
for i in a:
    print i,    # print tuple 00 01 10 11
```
#### itertools.repeat(object[,times])
```python
from itertools import repeat 

a = repeat(12, 3)
for i in a:
    print i,    # print 12 12 12 

a = repeat(12)
for i in a:
    print i,    # print  12 ...
```
