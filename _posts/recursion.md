title: 递归
date: 2015-10-21 11:48:31
categories: 算法
tags: [算法,]
---
函数在执行期间调用一个或多个函数本身的话就叫递归。
### 二分法查找
#### 原理
二分法主要是通过有序的序列查找想要找的值.
1. 确定序列的中间位置mid, 开始位置low和结束位置high.
2. 将查找的值与mid位上的值做比较, 如果相等则返回, 大于则把mid设为low, 小于则把mid设为high。
3. 在依次确定中间位置mid，循环计算到low > high.

#### 算法
时间复杂度为O(logN)
```python
def binary_search(data, target, low, high):
    if low > high:
        return
    else:
        mid = (low + high)//2
        if tartget == data[mid]:
            return data[mid]
        elif target < data[mid]]:
            return binary_search(data, target, low, mid-1)
        else:
            return binary_search(data, target, mid+1, high)

if __name__ == '__main__':
    L = range(1, 21)
    print binary_search(L, 5, 0, 19)
```
<!--more-->
### 二次递归相加
与二分法相似只是用于在递归时调用了两次
#### 原理
1. 确定起始位置与结束位置。
2. 如果起始位置比结束位置大或相等返回0, 如起始位置比结束位置小一位,返回起始位置值。
3. 如不满足上诉内容，取中间值mid, 递归自身。

#### 算法
时间复杂度为O(logN)
```python
def binary_sum(data, start, stop):
    if start >= stop:
        return 0
    elif start == stop - 1:
        return data[start]
    else:
        mid = (start + stop) // 2
        return binary_sum(data, start, mid) + binary_sum(data, mid, stop)

if __name__ == '__main__':
    L = range(1, 10)
    print binary_sum(L, 0, 9)
```
### 尾递归
尾递归是函数返回最后一个操作是递归调用，则该函数是尾递归。
#### 原理
递归是线性的比如`factorial`函数每一次调用都会创建一个新的栈(last-in-first-out)通过不断的压栈，来创建递归, 很容易导致栈的溢出。而尾递归则使用当前栈通过数据覆盖来优化递归函数。

#### 算法
阶乘函数`factorial`, 通过把计算值传递的方法完成了尾递归。但是python不支出编译器优化尾递归所以当递归多次的话还是会报错(学习用)。
```python
def factorial(n, x):
    if n == 0:
        return x
    else:
        return factorial(n-1, n*x)

print factorial(5, 1)  # 120
```
