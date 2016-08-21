title: datetime模块
date: 2015-04-21 10:30:41
categories: python
tags: [python, 标准库,]
---
## datetime模块
datetime模块提供对于日期和时间进行简单或复杂的操作. datetime 模块提供了一下的可用类型(Available Types).
>datetime.MINYEAR 和 datetime.MAXYEAR 模块常量表示`datetime`接受的范围
>class datetime.**date**: 一个理想化的日期, 提供**year**, **month**, **day**属性
>class datetime.**time**: 一个理想化的时间, 提供**hour**, **minute**, **second**, **microsecond**, **tzinfo**. 
>class datetime.**datetime**: 日期和时间的组合.提供**year**, **month**, **day**, **hour**, **minute**, **second**, **microsecond**, **tzinfo**.  
>class datetime.**timedelta**: 表达两个`date`,`time`和`datetime`持续时间内的微妙差异.
>class datetime.**tzinfo**: 时间对象的抽象基类. 

```python    
    from datetime import timedelta, datetime

    a = datetime.now()
    b = timedelta(days=7)

    # 7 days, 0:00:00
    # 2015-04-14 16:02:39.189000
    print b
    print a - b
```

<!--more-->
下面说具体说一下类和类的方法
### date类
一个`date`对象代表理想化的日期.
```python
    class datetime.date(year, month, day)
        # All arguments are required. Arguments may be ints or longs.
        # 所有参数都是必须的. 参数可能是 int 或 long.
        MINYEAR <= year <= MAXYEAR
        1<= month <= 12
        1<= day <= number of days in the given month and year.(随着月份和年份)

如果参数脱离给的范围会抛出, valueError.
#### 类方法 
>`date.today()`:返回当前的本地日期, 这等价于 `date.fromtimestamp(time.time())`.
Return the current local date. This is equvalent to `date.fromtimestamp(time.time())`.
    
    from datetime import date

    # print 2015-04-21
    print date.today()              
```

>`date.fromtimestamp(timestamp)`:根据提供的时间戳返回local `date`. 时间戳常用于对时间类型的存储.

```python
    import time
    from datetime import date

    # 1429587111.21
    # 2015-04-21
    print time.time()
    print date.fromtimestamp(time.time())
```

#### 类方法 
>`date.fromordinal(ordinal)`:根据提供的Gregorian日历返回`date`.(不做描述)

#### 类属性
>date.**min**: 返回 `date(MINYEAR, 1, 1)`.
>date.**max**: 返回 `date(MAXYEAR, 12, 31)`.
>date.**year**: 返回 **年**, MINYEAR和MAXYEAR之间
>date.**month**: 返回 **月**, 1到12月之间
>date.**day**: 返回 1到 n 之间.

```python
    d = date(2014, 4, 21)
    # 2014 4 21
    print d.year, d.month, d.day
```
#### 实例方法 
>date.**replace**(year, month, day):返回一个相同值的`data`对象, 除了这些参数给关键字指定新的值. 
>date.**timetuple()**: 返回一个time.struct_time对象.
>date.**toordinal()**: 返回一个Gregoian Calendar对象.
>date.**weekday()**: 返回day of the week. 星期一为`0`,星期日为`6`.
>date.**isoweekday()**: 返回day of the week. 星期一为`1`,星期日为`7`.
>date.**isocalendar()**: 返回一个三元组, (ISO year, ISO week number, ISO weekday).
>date.**isoformat()**: 返回 一个'YYYY-MM-DD'的字符串格式.
>date.**ctime()**: 返回一个字符串日期, `d.ctime()` 等同于 `time.ctime(time.mktime(d.timetuple()))`.
>date.**strftime(format)**: 返回一个字符串日期, 格式自定义.
 
```python
    d = date(2015, 4, 21)

    # 2015-04-21
    # 2015-04-21
    # 2015-04-22
    print d
    print d.replace()
    print d.replace(day=22)

    # time.struct_time(tm_year=2015, tm_mon=4, tm_mday=21, tm_hour=0, tm_min=0, tm_sec=0, tm_wday=1, tm_yday=111, tm_isdst=-1)
    print d.timetuple()

    # print 1
    # print 2
    print d.weekday()
    print d.isoweekday()

    # print 2015-04-21
    print d.isoformat()

    # print 21/04/2015
    print d.strftime('%d/%m/%y')
```

### datetime 类
`datetime` 对象是一个单一的对象, 包含所有`date`和`time`对象的信息.

```python
class datetime.datetime(year, month, day[, hour
                                        [, minute
                                        [, second
                                        [, microsecond
                                        [, tzinfo]]]]])
    # The year, month and day arguments are required.
    MINYEAR <= year <= MAXYEAR
    1 <= month <= 12
    1 <= day <= n
    0 <= hour < 24
    0 <= minute < 60
    0 <= second < 60
    0 <= microsecond < 10**6
```

#### 类方法
>`datetime.today()`: 返回当前本地`datetime`.随着 `tzinfo` None. 这个等同于`datetime.fromtimestamp(time.time())`.
>`datetime.now([tz])`: 返回当前本地日期和时间, 如果可选参数`tz`为None或没有详细说明,这个方法会像`today()`. 
>`datetime.utcnow()`: 返回当前的UTC日期和时间, 如果`tzinfo` None ,那么与`now()`类似.
>`datetime.fromtimestamp(timestamp[, tz])`: 根据时间戳返回本地的日期和时间.tz指定时区.
>`datetime.utcfromtimestamp(timestamp)`: 根据时间戳返回 UTC `datetime`.
>`datetime.fromordinal(ordinal)`: 根据Gregorian ordinal 返回`datetime`.
>`datetime.combine(date, time)`: 根据`date`和`time`返回一个新的`datetime`.
>`datetime.strptime(date_string, format)`: 根据`date_string`和`format`返回一个`datetime`.

```python
    from datetime import datetime

    # 2015-04-21 14:07:39.262000
    print datetime.today()

    # 2015-04-21 14:08:20.362000
    print datetime.now()

    # 1429596607.06
    # 2015-04-21 14:10:07.061000
    t = time.time() 
    print t
    print datetime.fromtimestamp(t)

    from datetime import datetime, date, time

    a = date(2015, 4, 21)
    b = time(14, 13, 34)

    # 2015-04-21 14:13:34
    print datetime.combine(a, b)
```

#### 实例方法
>`datetime.date()`: 返回相同**年月日**的`date`对象.
>`datetime.time()`: 返回相同**时分秒微秒**的`time`对象.
>`datetime.replace(kw)`: kw in [year, month, day, hour, minute, second, microsecond, tzinfo], 与`date`类似.
>其他方法可查看官方文档...

```python
    from datetime import datetime, date, time

    td = date(2015, 4, 21)
    n = time(14, 28, 30)

    # 2099-04-21 14:30:42.103000
    print datetime.now(0.replace(year=2099)
```

#### 类属性
>`datetime.min`: `datetime(MINYEAR, 1, 1)`.
>`datetime.max`: `datetime(MAXYEAR, 12, 31, 23, 59, 59, 999999)`.

#### 实例属性(read-only)
>`datetime.year`: 1 至 9999
>`datetime.month`: 1 至 12
>`datetime.day`: 1 至 n
>`datetime.hour`: In range(24). 0 至 23
>`datetime.minute`: In range(60).
>`datetime.second`: In range(60).
>`datetime.microsecond`: In range(1000000).

### time类
`time` 代表本地(一天内)时间.
```python
    class datetime.time([hour
                        [, minute
                        [, second 
                        [, microsecond
                        [, tzinfo]]]]])
        # All arguments are optional.
        # 所有参数都是可选的.
        0 <= hour < 24
        0 <= minute < 60
        0 <= second < 60
        0 <= microsesond < 10**6
```

time类就是对时间的一些操作,其功能类似与`datetime`.其实`date`和`time`就是对`datetime`中日期和时间的操作.

