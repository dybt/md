title: python 闭包
date: 2015-04-10 11:11:19
categories: python
tags: [python, 闭包与装饰器]
---
在一个函数内定义内部函数,且内部函数使用外部函数作用域,则称为`闭包`.

### 闭包例子
```python
def add(x):
    def addx(y):
        return x + y
    return addx

foo = add(5)
print foo(1)  # print 6
print foo(10) # print 15
```
<!--more-->
上面就是一个简单的闭包的例子，addx这个函数引用了外部函数的作用域. 而内部函数参数改变foo的返回值也发生了改变.
```python
def clock():
    count = 0
    def counter():
        count += 1
        return count
    return counter
foo = clock()
for i in range(3):
    print foo(),

def counter():
    nonlocal count
```
按理说会打印出 1, 2, 3 .但是python 会报UnboundLocalError的错误,没有定义变量直接引用.其实这是python2.X的一个bug在python3.X里引入了关键字nonlocal通过这个关键字就可以解决问题
