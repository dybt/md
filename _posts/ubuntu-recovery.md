title: ubuntu使用extundelete恢复rm删除的文件
date: 2016-02-03 23:19:40
categories: 生活
tags:
- extundelete
- 恢复
---
![图片来自互联网](http://7sbxbm.com1.z0.glb.clouddn.com/extundelete.jpg)
*利用extundelete软件来恢复不小心删除的文件*
<!-- more -->
<!-- toc -->
### 一、情况
刚刚在学习C语言编程的时候，写完源代码，编译时在终端输错了命令。原来要输入:

	rm a.out && gcc sort_binary.c

结果一恍惚把`&&`给漏打进去了，变成这样的命令：

	rm a.out gcc sort_binary.c

这样就把我的`sort_binary.c`的源文件给删除了，悲剧。
Google了下，发现了`extundelete`这个软件。并参考了这篇[博客](http://ixdba.blog.51cto.com/2895551/1566856)。

### 二、安装extundelete
	sudo apt-get install extundelete
### 三、恢复
我的是用到恢复单个文件，如果要用到其他用法请google或者看那篇参考博客。

	sudo extundelete /dev/sda7 --restore-file /home/dg/c/sort_binary.c

执行完这个命令并提示成功，会生成一个`RECOVERED_FILES`文件夹，里面就存放着刚刚恢复的文件了。
![](http://7sbxbm.com1.z0.glb.clouddn.com/extundelete1.png)
![](http://7sbxbm.com1.z0.glb.clouddn.com/extundelete2.png)


