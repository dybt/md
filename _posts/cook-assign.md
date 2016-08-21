title: 迭代对象赋值给多个变量
date: 2015-06-19 17:47:15
categories: python
tags: [python3,]
---
本文是把[python3 cookbook](http://python3-cookbook.readthedocs.org/zh_CN/latest/copyright.html)中一些有用的特性进行总结。

### 问题
如果对一个迭代对象的元素进行赋值的话，就必须保持与对象元素相同的变量。不然会报错

```bash
# 正确的例子
a, b, c = [1, 2, 3]

# 错误的例子
a, b, c = [1, 2]
a, b = [1, 2, 3]
```
如果只有几个元素的话我想不会有人出错, 但是当元素数量多时就需要这么做

```bash
a, *b = '1231222222222222222222'

print(a)  # 1
print(b)  # 2312222222 ...
```
如果想要指定信息可以
```bash
a, *_, b = '3dddddddddddd4'

print(a) # 3
print(b) # 4
```
