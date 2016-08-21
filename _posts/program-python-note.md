title: python学习笔记
date: 2016-01-02 09:36:45
categories: 编程
tags:
- python
- 编程
---
![图片来至网络](http://7sbxbm.com1.z0.glb.clouddn.com/programnote.jpg)
*[编辑中]我自己开学学习python的一些笔记，零基础类型的。*
<!-- more -->
### 目录
<!-- toc -->
### 1.ubuntu下环境设置
#### 安装IDLE[python3.5]
	sudo apt-get install idle-python3.5
### 2.[python2.7,windows7]*.py文件中包含中文字符不能保存
#### 方法一
在.py文件开头加入`#coding=gbk`
#### 方法二[我目前使用的方法]
在.py文件开头加入`#coding=utf8`,在文中每个有中文的字符串前加上字母`u`
![](http://7sbxbm.com1.z0.glb.clouddn.com/programpython_note_wxpython_encode.png)
### 3.[python2.7,windows7]UnicodeEncodeError: 'ascii' codec can't encode characters in position
在文件开头加入三行
``` python
import sys
reload(sys)
sys.setdefaultencoding('utf8')
``` 

### 4.python资源链接
#### a.[python中文学习大本营](http://www.pythondoc.com/)
#### b.[python中文官方文档](http://python.usyiyi.cn/)
### 5.scrapy安装
*简介：[scrapy](http://scrapy.org/)网络爬虫。*
我的系统是Lubuntu 15.04,参考[scrapy官方教程](http://doc.scrapy.org/en/latest/intro/install.html)来安装的。
#### a.安装依赖
	sudo apt-get install python-dev python-pip libxml2-dev libxslt1-dev zlib1g-dev libffi-dev libssl-dev

#### b.通过pip安装scrapy
	sudo pip install scrapy

### 6.python通过socks5代理上网
github下载的模块 [PySocks](https://github.com/Anorov/PySocks)
'''python
import socks
import socket
socks.set_default_proxy(socks.SOCKS5, "127.0.0.1", 10808)
socket.socket = socks.socksocket
'''
