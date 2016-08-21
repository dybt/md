---
title: 字符串匹配
date: 2016-05-20 12:33:12
tags: [算法, python]
---

### 朴素匹配算法
朴素匹配算法是对目标字符串和模板字符串的一一匹配。如果匹配得上,下标向右移一位, 否则清空并重新开始匹配。
```python
target = 'abb aba'
pattern = 'aba'

def match(target, pattern):
    i = j = 0
    n, m = len(target), len(pattern)
    while i < n and j < m:
        # 如果字符相等则目标和模板的下标都向右移
        if target[i] == pattern[j]:
            i, j = i+1, j+1
        else:
            # 如果字符不相等则目标下标切换到不相等的下标
            # 模板下标移动到初始下标
            i = i - j + 1
            j = 0
    if j == m:
        return i - j
    return -1
```
<!--more-->
把上面的加上print后打印一遍
```python
#修改的地方
else:
    i = i -j + 1
    j = 0
    print(target[i], pattern[j], i, j)

# 打印结果
b a 1 0
b a 2 0
  a 3 0
a a 4 0
```
循环会一直到相等的匹配值, 这个方法效率低下，主要是在不匹配时会重新把模板字符循环一次。最多可能会出现 m * (n-m +1)次。m是模板字符的长度，n-m + 1是排除不等字符的次数。

### KMP 算法
kmp是通过已知匹配的字符进行移位的算法，比如上面的`abb` 中跟`abc`比较的话 `ab`是已知的。
```python
def match(target, pattern):
    i = j = 0
    n, m = len(target), len(pattern)
    while i < n and j < m:
        # 如果字符相等则目标和模板的下标都向右移
        if if j == -1 and target[i] == pattern[j]:
            i, j = i+1, j+1
        else:
            # 这里通过next 函数来判断位移个数
            i = i - j + pattern_next(pattern[:j])
            j = 0
    if j == m:
        return i - j
    return -1


def pattern_next(s):    
    prefix = [s[:i+1] for i in range(len(s)-1)]
    suffix = [s[i+1:] for i in range(len(s)-1)]
    l = list(set(prefix) & set(suffix))
    return len(l)
```

### 参考
1. [数据结构与算法(python)](http://www.math.pku.edu.cn/teachers/qiuzy/ds_python/courseware/DS3-string-1.pdf)
2. [kmp算法python实现](http://www.cnblogs.com/goodspeed/p/3295456.html)
3. [kmp算法原理](http://www.ruanyifeng.com/blog/2013/05/Knuth%E2%80%93Morris%E2%80%93Pratt_algorithm.html)
