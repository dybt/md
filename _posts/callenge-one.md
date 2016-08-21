title: 挑战 例题 (1)
date: 2015-04-07 17:34:00
categories: python
tags: [python,]
---
此类文章作为本人在[python tip][1]上的在线挑战python例题。
[1]: http://www.pythontip.com   "python tip"

### 第一题: give you two var a and b, print the value of a+b, just do it!
根据提议，给出两个变量 a 和 b 并打印出 a+b的值.
``` bash
a, b = 1, 2
print a + b
```
当然也可以这么做
``` bash
a = 1
b = 2
print a + b
```
<!--more-->
### 第二题: 给你一个list, 如 L = [2, 8, 3, 5], 对L进行升序排序并输出。
``` bash
L = sorted(L)
print L
#或
# sort() 内置函数会对列表自身排序而 sorted() 会生成一个新的排序列表 
L.sort()
print L
```
### 第三题: 给你一个字符串a， 如a = '12345', 对a进行逆序输出。
``` bash 
# 通过步进来逆序输出字符串。
a = a[::-1]
print a
```
### 第四题: 给你一个字典a = {1:1, 2:2, 3:3}, 输出字典a的key ,以',' 连接，如 '1,2,3'。
``` bash
# dict.keys() 会以list返回字典的key.而join会把list按,字符串',' 连接起来。
print ','.join(a.keys())
```
### 第五题: 给你一个字符串a, 输出字符串奇数位置的字符串,如 a = '12345', 则输出 '135'
``` bash
# 通过列表解析(也称列表推导式)来判断下表选取奇偶数。
print ''.join([a[x] for x in range(len(a)) if x % 2 == 0])
```
