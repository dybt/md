---
layout: post
title: "OS X 下如何实现批量重命名文件"
date: 2013-01-13 01:01
comments: true
tags:
- 批量重命名
- Automator
categories: Automator
---

Windows和Linux下批量重命名都是非常简单的（可以参考[批量重命名文件 - Linux Wiki](http://linux-wiki.cn/wiki/zh-hk/%E6%89%B9%E9%87%8F%E9%87%8D%E5%91%BD%E5%90%8D%E6%96%87%E4%BB%B6)）。OS X当然也可以像Linux那样用命令行，但是又要通过[Homebrew](http://mxcl.github.com/homebrew/)或者[MacPorts](http://www.macports.org/)，对于很多人来说可能比较麻烦或者不会用命令行。我一直在想OS X这么高级的操作系统难道没有更好用的批量重命名的方法吗？答案是肯定的~
<!--more-->
之前我一直没有用过系统自带的Automator这个强大的工具，更是没有想到要用，后来看了@ibuick的[《OS X Mountain Lion高手进阶》](http://ibuick.com/online-store)之后做一些操作之前总是先想一下能不能用Automator来代替我自动批量完成。下面了介绍一下怎么来通过Automator来做一个批量重命名的工具吧。

1. 打开Automator，在弹出的“选取文稿类型”框中选择“服务”，设置 【“服务”收到选定的】选“文件或文件夹”、【位于】选“Finder”；
2. 在资源库中找到“给 Finder 项目重命名”，双击或拖到右边“工作流组装区”，**记得勾选“工作流程运行时显示次操作”**；
3. 保存服务，将其命名为“批量重命名”

最后让大家看一下设置截图：

[{% img http://farm9.staticflickr.com/8330/8372891419_8628bd032a_c.jpg Automator设置 %}](http://www.flickr.com/photos/92204252@N08/8372891419/)

以后如果想给文件批量重命名的时候只需要选择需要重命名的文件，然后选择“” -> “服务”或者点击右键选择服务，找到“批量重命名”就可以了~

[{% img http://farm9.staticflickr.com/8329/8372922251_cc83e25b1e_z.jpg 执行效果图 %}](http://www.flickr.com/photos/92204252@N08/8372922251/)

最后附上我制作的[batch-rename.workflow.zip](/uploads/files/batch-rename.workflow.zip)，你可以下载之后解压，然后双击安装。