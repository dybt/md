title: python collections 模块
date: 2015-04-15 11:50:06
categories: python
tags: [python, 标准库,]
---
**collections**模块在pyhton2.4版本中被引入, 提供了一些特殊容器数据类型, 替换选择dict, list, set and tuple.

## Counter
Counter用来记录值的出现次数.这是一个无序的集合类型以字典的kw形式存储, 元素为key,计数为值
    
Counter多种创建方法
```python
c = Counter()
c = Counter('dkdkjfa')
c = Counter({'mink': 4, 'cats':3})
c = Counter(mink=4, cats=3)

# 用del方法删除键
del c['mink'] 
```
<!--more-->
现在试一下Counter
```python    
c = Counter()
print c, type(c)            # print Counter() <class 'collections.Counter'>

#修改一下
c = Counter('dkdkjfa')

print c, c['k']             # print Counter({'k':2, 'd':2, 'j':1, 'f':1, 'a':1}) 2
# 试着输入没有的key
print c['m']                # print 0
```
没错与字典不同他会返回一个counter的类对象,并输出不存在的key会返回0.
### elements
按计数的内容返回一个任意顺序的iterator. 如果计数小于1则不返回.
```python    
c = Counter({'a':1, 'b':2, 'c':3, 'd': -2})
print list(c.elements())    # print [a, c, c, c, b, b]
```
### most_common
返回一个TopN的二元祖列表, 如果n没有被指定,则返回所有元素.
```python
c = Counter('aaabbbc')
print c.most_common(2)       # print [('a', 3), ('b', 3)] 
print c.most_common()      # print [('a', 3), ('b', 3), ('c', 1)] 
```
### subtract
元素会减去来自一个iterator或其他映射与计数.有点像**dict.update()**但**subtract**用来替换他们.
```python
c = Counter(a=4, b=2, c=0, d=-2)
d = Counter(a=1, b=2, c=3, d=4)
c.subtract(d)
print c                     # print Counter({'a':3, 'b':0, 'c':-3, 'd':-6})
```
### fromkeys
无语(官网:This class method is not implement for Counter objects.)
### update 
与substract相反,可以试一下
```python    
c = Counter('abc')
d = Counter('acd')
c.update(d)
print c                     # print Counter({'a':2, 'b':1, 'c':2, 'd':1})
```
### Counter 常用操作
```python
sum(c.values())               # 所有计数的总和
c.clear()                     # 重置对象(reset all count)
list(c)                       # c中的键转换为list
set(c)                        #     ...转换为set
dict(c)                       #     ...转换为dict
c.items()                     # 转换为(arg, count)的list
Counter(dict(list_of_pairs))  # 转换自(arg, count)的list到counter
c.most_common()[:-n-1:-1]     # sorted计数 
c += Counter()                # 移除0和负数
```
## deque
使用`list`存大量数据时, 插入和删除会很慢. deque高效的实现插入和删除操作的双向列表.

### append 和 appendleft
* append 在队列尾部添加对象
* appendleft 在队列首部添加对象

```python    
from collections import deque

d = deque('mink')

for i in d:
    print i.upper(),        # print M I N K

d.append('ok')
print d                     # deque(['m', 'i', 'n', 'k', 'ok'])

d.appendleft('oh')       
print d                     # deque(['oh', 'm', 'in', 'n', 'k', 'ok'])
```

### pop 和 popleft
* pop 从尾部删除并返回一个元素, 如果没有元素提出,则返回IndexError
* popleft 从首部删除并返回一个元素, 如果没有元素提出,则返回IndexError

```python
print d.pop()               # print 'ok'
print d                     # deque(['oh', 'm', 'in', 'n', 'k'])

print d.popleft()           # print 'oh'
print d                     # deque(['m', 'i', 'n', 'k'])
```
### extend 和 extendleft
* extend 从尾部扩展元素
* extendleft 从首部扩展元素

```python
d.extend('abc')
print d                     # deque(['m', 'i', 'n', 'k', 'a', 'b', 'c'])

d.extendleft('xyz')
print d                     # deque(['x', 'y', 'z', 'm', 'i', 'n', 'k', 'a', 'b', 'c'])
```

### reverse 和 rotate
* reverse 会反转队列, 返回值为None. 如: d.reverse() is None
* rotate 会按步进(step) 旋转队列,默认为1. 正数left-to-right,负数right-to-left.

```python
d = deque('mink')
d.reverse()
print d                     # deque(['k', 'n', 'i', 'm'])

d.rotate(1)                 # deque(['m', 'k', 'n', 'i'])
d.rotate(-2)                # deque(['n', 'i', 'm', 'k'])
```
## OrderedDict
因为dict是无序的所以跟进dict排序时可以用到**OrderedDict**
```python
from collections import OrderedDict 

d = OrderedDict(a=1, b=2, c=3)

print d                         # print OrderedDict([('a', 1), ('c', 3), ('b', 2)])
print d.keys()                  # print ['a', 'c', 'b']
```

纳尼？不是说是有序的么. 其实OrderedDict不是按着key排序而是通过插入排序,那么问题来了上面. a=1, b=2, c=3也是按着顺序啊. **OrderedDict**会把a=1,b=2,c=3这样的看成一个字典因为字典是无序的所以他的排序就不会按预期的那样,上面的应该这么写
```python
d = OrderedDict([('a',1), ('b',2), ('c',3)])

print d                         # print OrderedDict([('a', 1), ('b', 2), ('c', 3)])
print d.keys()                  # print ['a', 'b', 'c']

# 也可以这样
d = OrderedDict()
d['a'] = 1
d['b'] = 2
c['c'] = 3
```
OrderedDict是插入顺序的, 而不是键排序

## defaultdict
类似于字典，`defaultdict`能接受一个内建类型为参数制定值得类型。
```python
d = defaultdict(list)

# 因为指定了类型为list所以d的值为列表类型
d['a'].append(1)       
print d             # defaultdict(<type 'list'>, {'a': [1]})

d['a'].append(2)    # defaultdict(<type 'list'>, {'a': [1, 2]})
d.items()           [('a', [1, 2])]

# 还可以这样
d = {}
d.setdefault('a', []).append(1)

print d             # {'a': [1]}
d.setdefault('a').append(2)
print d             # {'a': [1, 2]}
```
