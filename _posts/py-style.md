title: 写出符合python的代码风格
date: 2015-06-24 11:58:35
categories: python
tags: [style, python]
---
写出符合`python`风格和效率的代码，本文内容从网上收录而来加以整理。

### 好的习惯很重要
我也经常会写一些恶心到自己的代码, 回头看的时候真心没法看，又丑又长,效率也低。所以是时候改变下自己的代码了

>`python`有很多方便的用法，但是如果使用这些会让你的代码晦涩难懂，请不要使用。`python`讲究越明确越好，可提高代码的可维护性， 如果必须使用的情况下，请写好注释。(= =) 其实我是对我说的。

**Bad**
```bash
def make_complex(*args):
    x, y = args
    return dict(**locals())
```
**Good**
```bash
def make_complex(x, y):
    return {'x': x, 'y': y}
```
<!--more-->

>每行仅声明一个.

**Bad**
```bash
print 'one'; print 'two'

if x == 1: pirnt 'one'

if <complex comparison> and <other complex comparison>:
    # do something
```

**Good**
```bash
print 'one'
print 'two'

if x == 1:
    print 'one'

cond1 = <complex comparison>
cond 2 = <other complex comparison>
if cond1 and cond2:
    # do something
```

> 你不应该显示的比较一个值是否 `True`,`None`,`0`, `空` ... 

**Bad**
```bash
if attr == True:
    print 'True!'

if attr == None:
    print 'attr is None!'
```

**Good**
```bash
if attr:
    print 'attr is truthy!'

if not attr:
    print 'attr is falsey!'

if attr is None:
    print 'attr is None!'
```

>当你想要使用一个字典的元素时，不要使用`dict.has_key()`.应该`x in dict`等语法或使用默认的`dict.get()`。

**Bad**
```bash
d = {'hello': 'world'}
if d.has_key('hello'):
    print d['hello']
else:
    print 'default_value'
```

**Good**
```bash
d = {'hello': 'world'}

print d.get('hello', 'default_value')   # print 'world'
print d.get('thingy', 'default_value')  # print 'default_value'

# or 
if 'hello' in d:
    print d['hello']
```

>操作列表的时候(比如过滤列表),请使用列表解析(也叫列表推导式)和`map()`等内置函数.

**Bad**
```bash
a = [3, 4, 5]
b = []
for i in a:
    if i > 4:
        b.append(i)
```

**Good**
```bash
a = [3, 4, 5]
b = [i for i in a if i > 4]
# or 
b = filter(lambda x: x> 4, a)
```

**Bad**
```bash
a = [3, 4, 5]
for i in range(len(a)):
    a[i] += 3
```

**Good**
```bash
a = [3, 4, 5]
a = [i + 3 for i in a]
# or 
a = map(lambda i: i+ 3, a)
```

使用`enumerate`来计数
```bash
a = [3, 4, 5]
for i, item in enumerate(a):
    print i, item

# 0 3
# 1 4
# 2 5
```

> 打开文件使用, `with` 处理文件将自动为你关闭文件

**Bad**
```bash
f = open('file.txt')
a = f.read()
print a
f.close()
```

**Good**
```bash
with open('file.txt') as f:
    for line in f:
        print line
```

> 需要折行的,请使用`()` 虽然可以使用`\`但是有时会出现意想不到的错误

**Bad**
```bash
my_very_big_string = """For a long time I used to go to bed early. Sometimes, \
    when I had put out my candle, my eyes would close so quickly that I had not even \
        time to say “I’m going to sleep.”"""

from some.deep.module.inside.a.module import a_nice_function, another_nice_function, \
    yet_another_nice_function
```

**Good**
```bash
my_very_big_string = (
    "For a long time I used to go to bed early. Sometimes, "
        "when I had put out my candle, my eyes would close so quickly "
            "that I had not even time to say “I’m going to sleep.”"
            )

from some.deep.module.inside.a.module import (
    a_nice_function, another_nice_function, yet_another_nice_function)
```

本人一直觉得自己写的非常符合`pep8`的规范, 昨天心血来潮试了一下问题多多
```bash
pip install pep8

# 使用pep8 检测你的代码
pep8  <filename>.py
```
这样就会提示你哪些代码不符合规范. 下面是我常犯的错误

* `# _*_coding:utf-8_*_`  注释后面没有留空格, 比如第一行
* 行末加了一个空格
* 函数没有分隔 2行， 而是只分了一行
* 空行中添加空格符(有时候顺手就打上了)
* 文件最后一行是多余行
* 参数格式用视觉效果排的


