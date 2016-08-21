title: python time模块
date: 2015-04-23 15:06:12
categories: python
tags: [python, 标准库,]
---
`time`模块是包含各方面对时间操作的函数. 尽管这些常常有效但不是所有方法在任意平台中有效. `time`用`struct_time`表示时间

    import time

    # time.struct_time(tm_year=2015, tm_mon=4, tm_mday=24, 
                        tm_hour=14, tm_min=17, tm_sec=26, 
                        tm_wday=4, tm_yday=114, tm_isdst=0)
    # 2015
    print time.localtime()
    print time.localtime().tm_year

<!--more-->
### 函数
>`time.time()`: 返回一个时间戳
>`time.asctime([t])`: 转换`gmtime()`和`localtime()`返回的元组或`struct_time`为`string`.
>`time.clock()`: 在第一次调用的时候, 返回程序运行的时间. 第二次之后返回与之前的间隔.
>`time.ctime([secs])`: 将时间戳转换为时间字符串, 如没有提供则返回当前的时间字符串,并与`asctime(localtime())`一样.
>`time.gmtime([secs])`: 将时间戳转化为, UTC 时区的`struct_time`.
>`time.localtime([secs])`: 类似`gmtime()`但会把他转换成本地时区.
>`time.mktime(t)`: `struct_time` 转化为时间戳.
>`time.sleep(secs)`: 线程推迟指定时间, 以秒为单位.
>`time.strftime(format[,t])`: 根据参数转换一个`sturc_time`或元组为字符串.
>`time.strptime(string[, format])`: 与`strftime`相反,返回一个`struct_time`.
    
    import time

    # Fri Apr 24 06:39:34 2015
    print time.asctime(time.gmtime())

    # 0.0
    # None
    # 1.01136392961 因计算机而异
    print time.clock()
    print time.sleep(1)
    print time.clock()

    # Fri Apr 24 14:42:07 2015
    print time.ctime()

    # 2015-04-24
    print time.strftime('%Y-%m-%d', time.localtime())
    # 1429857836.0
    print time.mktime(time.localtime())

time模块中常用的格式化字符串
>`%y` 两位数的年份 `00 ~ 99`.
>`%Y` 四位数的年份 `0000 ~ 9999`
>`%m` 月份 `01 ~ 12`.
>`%d` day `01 ~ 31`.
>`%H` 时 `00 ~ 23`.
>`%I` 时 `01 ~ 12`.
>`%M` 分 `00 ~ 59`.
>`%S` 秒 `00 ~ 61`.


