title: 冒泡排序
date: 2015-07-09 11:42:26
categories: 算法
tags: [算法, 排序]
---
简单的冒泡排序, 就是两个元素之间互相比较大的(或小的)排到最后。
```python
import random 

data = random.sample(range(1, 8), 7)
print 'data: ', data

for i in range(len(data)-1):
    for j in range(len(data)-1-i):
        if data[j] > data[j+1]:
            data[j], data[j+1] = data[j+1], data[j]
 
print 'data: ', data
```
打印结果
```python
data:  [4, 6, 5, 2, 7, 3, 1]
data:  [1, 2, 3, 4, 5, 6, 7]
```
