---
title: java问题定位技术
date: 2016-08-01 21:11:06
tags:
 -Java
 -Java进阶
categories:
 -Java
 -Java进阶
---

## 简介

  &nbsp;&nbsp;&nbsp;&nbsp; Java 定位问题主要是解决Java程序的，可以通过一些命令查看一些Java内存的一些东西，
   从而确定出问题所在和系统的瓶颈所在。

### jps
 jps 命令可以打印出所有的Java进程，类似于linux下ps命令打印出Java所有进程的信息。
 参数:
 >  -l 打印详细的类的信息，包括类的全称名。
 >  -m 打印出虚拟机的输入的参数,即args。
 >  -v 打印出虚拟机的具体参数。
 
### jstack

> 既然jps能够具体的打印出Java进程的具体的pid，那么jstack就可以生成某一时刻的内存快照，通过jstack命令
> 生成的内存快照，可以分析Java进程的内存信息。

> 快照中的一些信息会具体再写一篇博文进行叙述。

jstack <pid> 生成pid进程的java内存信息

### jmap

jmap命令可以通过可以对内存的一些信息进行映射或者堆的内存细节。

> -heap <pid> 打印出内存堆的信息。
> -histo <pid> 打印堆的柱状图。其中包括每个Java类、对象数量、内存大小、还有类的权限定名。
> -histo:live 先进行一次内存回收，然后再打印-histo的东西。
> -dump:[live]format=b,file=<filename> 以proof二进制格式转储Java堆到指定filenanme
文件中。live子选项是可选的。可以通过jhat工具来读取生成的文件。

### jhat

jhat 是一个工具类，通过这个类可以查看dump文件。也可以通过浏览器来进行查看。

### jinfo

jinfo 是查看某个Java进程的JVM的具体的参数的。


### javap

javap是生成java简单汇编代码的工具。



