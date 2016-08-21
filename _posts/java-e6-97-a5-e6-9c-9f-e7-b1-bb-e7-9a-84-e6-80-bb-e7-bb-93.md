---
title: Java日期类的总结
id: 249
categories:
  - Java date
  - Java复习
date: 2016-05-19 15:42:27
tags:
    - Java
---

&nbsp;&nbsp;&nbsp;&nbsp;最近在做一个安卓的小玩意，因为用到了日期的一些操作，便写下一边博客，提醒大家如何少走坑路，正确使用正确的日期。

&nbsp;&nbsp;&nbsp;&nbsp;Java 8中已经增加了time包了，但是如果由于条件的限制不能使用Java8的time包的话，那就使用Calendar类和SimpleDateFormat类来进行时间的操作了

&nbsp;&nbsp;&nbsp;&nbsp;之前一直想用Date类，后来发现Date类好多方法都废弃了。

&nbsp;&nbsp;&nbsp;&nbsp;原因是Date中一些操作不方便，例如创建一个Date的时候，传入的year的值是要创建的年份-1990，而月份的值也是要减1的，这就不太符合人们的一些习惯。例如我们打算创建2016-05-20 我们的构造语句如下:

[java]Date date = new Date(126, 4, 20);[/java]

&nbsp;&nbsp;&nbsp;&nbsp;于是找到一个更好用的类，Calendar类，这个类可以获取当前日期，也可以自定义日期。

&nbsp;&nbsp;&nbsp;&nbsp;初始化一个Calendar类的语句:

&nbsp;&nbsp;&nbsp;&nbsp;[java]Calendar calendar = Calendar.getInstance();[/java]

如果我们想要自定一个一个时间,初始化Calendar类之后，可以通过[java]calendar.set(int key, int value);[/java] 的方法来进行时期的设定。

其中key值包含Calendar.YEAR等枚举类型。

要获取一个日期的年月日也是相当的方便，通过[java]calendar.getXXX()[/java]方法可以获得年月日

除了年月日的获取，对时间的操作也是类似的。

说了时间的创建和获取以及修改，我们再来谈一谈如何对时间进行格式化，这里用到时间格式化的类，SimpleDateFormat

创建方式如下:

[java]SimpleDateFormate simpleDateFormat = new SimpleDateFormat(&amp;quot;yyyy-MM-dd HH:mm:ss&amp;quot;);[/java]

对日期进行格式可以通过[java]simpleDateFormate.format()[/java]方法进行格式化，但是切忌不能把Calendar对象之间传入，可以通过calendar.getTime()获取当前的Date对象，从而实日期的格式化。