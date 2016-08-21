title: 挑战 例题 (2)
date: 2015-04-09 10:21:47
categories: python
tags: [python, ]
---

### 第六题: 输出所有100以内的素数, 素数之间以空格区分。
``` bash
# 在加一行print 才能运行通过
L = [x for x in range(2, 101) if not [y for y in range(2, x) if x%y == 0]]
for i in L:
    print L,
print 
```
<!--more-->
### 第七题: 已知矩形长a, 宽b, 输出其面积和周长，以一个空格隔开
``` bash
print a * b, 2 * (a+b)
```

### 第八题: 给你一个list, 如 L = [0, 1, 2, 3, 4] 输出L的中位数
``` bash
# 中位数是指 对已排序的数集取其中间数，数集为偶数 取中间两数的平均
print sorted(L)[len(L)/2] if len(L) % 2 != 0 else \
        (sorted(L)[Len(L)/2] + sorted(L)[len(L)/2 -1 ])/2.0
```

### 第九题: 给你两个正整数a和b, 输出它们的最大公约数。
``` bash
print max([x for x in range(1, a+1) if x in [y for y in range(1, b+1) if b%y == 0]])
```

### 第十题: 给你两个正整数a和b， 输出它们的最小公倍数.
``` bash
print min([x for x in range(a, a*b+1) if x%a==0 and x%b==0])
```

### 附加题: 利用map/reduce 函数实现数的阶乘 如 5!+4!+3!+2!+1!.
``` bash 
print map(lambda x: reduce(lambda y,z: y*z, range(1, x+1)), range(1,6))
# print [1, 2, 6, 24, 120], 所以在用reduce合起来
print reduce(lambda a, b: a+b, map(lambda x: \
        reduce(lambda y, z: y*z, range(1, x+1)),range(1,6)))
```
### 附加题: 使用filter函数 实现素数
``` bash 
print filter(lambda x: not [x%i for i in range(2,x) if x%i==0], range(2, 101))
```
