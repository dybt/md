title: 时间日期操作
date: 2015-09-09 15:20:08
categories: python
tags: [python, 标准库, 时间]
---
有很多时候需要对时间进行操作，本文将记录对日期，时间戳，时间字符串之间的转换。

### 日期类型转换为时间字符串
```python
import time
import datetime

# datetime.datetime(2015, 9, 9, 15, 25, 24, 927246)
today = datetime.datetime.today()

# '2015-09-09 15:25:24'
today.strftime('%Y-%m-%d %H:%M:%S')
```
<!--more-->
### 时间字符串转换为日期类型
datetime 接受的参数必须为int类型
```python
_datetime = '2015-09-03 15:32:23'

_date = _datetime.split()[0]
_time = _datetime.split()[1]

year, month, day = _date.split('-')
hour, minute, second = _time.split(':')

# datetime.datetime(2015, 9, 3, 15, 32, 23)
dt = datetime.datetime(
        int(year), int(month), int(day),
        int(hour), int(minute), int(second)
)
```

### 日期转换为时间戳
time.mktime 会把一个日期转换为时间戳但是必须输入九元素元组。
```python

# datetime.datetime(2015, 9, 9, 15, 25, 24, 927246)
today  = datetime.datetime.today()

# 1441783524.0
time.mktime((
        today.year, today.month, today.day,
        today.hour, today.minute, today.second,
        0, 0, 0
))
```
### 时间戳转换为日期形式
```python
b = 1441783524.0

# datetime.datetime(2015, 9, 9, 15, 25, 24, 927246)
datetime.datetime.fromtimestamp(b)
```
### 时间戳转换为字符串
time.localtime 会返回一个struct_time类型
```python
b = 1441783524.0

# time.struct_time(tm_year=2015, tm_mon=9, tm_mday=9, tm_hour=15, tm_min=25, tm_sec=24, tm_wday=2, tm_yday=252, tm_isdst=0)
_t = time.localtime(b)

# 2015-09-09 15:25:24
time.strftime('%Y-%m-%d %H:%M:%S', _t)
```

### 本地时间与utc时间转换
```python
utc  = '2016-04-18T13:04:41.413Z'
utc_datetime = datetime.datetime.strptime(utc, '%Y-%m-%dT%H:%M:%S.%fZ')

# datetime.datetime(2016, 4, 18, 13, 4, 41, 413000)
print(utc_datetime)

local = utc_datetime + datetime.timedelta(hours=8)
local = local.strftime('%Y-%m-%d %H:%M:%S')
# '2016-04-18 21:4:41'
print(local)

```
