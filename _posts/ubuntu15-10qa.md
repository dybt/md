title: ubuntu15.10使用过程遇到问题及解决方法
date: 2016-01-12 06:01:38
categories: 生活
tags:
- ubuntu
---
![](http://7sbxbm.com1.z0.glb.clouddn.com/ubuntu.png)
*我自己使用ubuntu15.10日程过程中遇到的问题和解决方法*
<!-- more -->
目录
<!-- toc -->
#### 1.修改默认语言
把默认中文改成英语，如果我们安装的是中文版，在不进入图形界面只进入命令终端(tty1~tty6)时，由于命令终端默认不支持中文，中文会显示为菱形方块。
编辑`/etc/defualt/locale`,将里面内容改为：
```
LANG="en_US.UTF-8"
LANGUAGE="en_US:en"
```
#### 2.安装amd显卡专有驱动后不能进入系统
安装amd显卡专有驱动后开机，卡在ubuntu logo的位置不能进入系统。
重启，选择高级选项开机项，进入选择recovery模式，进入后选择root shell，然后把专有驱动卸载就好了。卸载命令如下：
```
apt-get autoremove --purge fglrx*
```
#### 3.U盘安装，非efi主板
正常用U盘代替光盘的安装Ubuntu ，用ultraISO或者unetboot这两款软件就可以了。在安装15.10版本时，比较旧的电脑不支持efi，无法从u盘启动。我试了用Win32DiskImager来制作启动U盘，就可以在旧电脑上启动了。
#### 4.add-apt-repository: command not found
```
sudo apt-get install python-software-properties
```
