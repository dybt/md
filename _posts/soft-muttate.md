title: 软件推荐之Mutate
date: 2016-01-11 02:19:19
categories: 软件
tags:
- muttate
---
![图片duang自github](http://7sbxbm.com1.z0.glb.clouddn.com/softmutate.png)
*Mutate是一款类似Alfred(OS X)的启动器软件，通过组合快捷键快速执行各式任务。*
<!-- more -->
安装、使用等详见[mutate](https://github.com/qdore/Mutate)项目主页。
### 目录
<!-- toc -->
### 一、安装
#### 1.安装编译工具和依赖
	sudo apt-get install build-essential qt5-qmake qt5-default libgtk2.0-dev libqt5x11extras5-dev libboost-regex-dev
#### 2.安装git[如果你已经安装了，跳过此步奏]
	sudo apt-get install git
#### 3.下载和编译mutate
```
git clone https://github.com/qdore/Mutate.git
cd Mutate/src
qmake
make
```
`
#### 4.安装mutate
```
sudo make install
cd ..
sudo cp info/mutate.png /usr/share/icons
sudo cp info/Mutate.desktop /usr/share/applications
mkdir -p ~/.config/Mutate
cp -R config/* ~/.config/Mutate
chmod -R a+x ~/.config/Mutate/scripts
chmod -R a+w ~/.config/Mutate
sed -i "s|{home}|$HOME|g" ~/.config/Mutate/config.ini
```
`
### 二、使用
#### 1.启动mutate
在终端[terminal]输入`mutate`打开mutate，首次启动关闭后用`Ctrl+D`快捷键启动mutate对话框。
#### 2.查找启动程序
直接在对话框里面输入软件的名字
![图片来至github](http://7sbxbm.com1.z0.glb.clouddn.com/softmutate.png)
#### 3.查找文件
在对话框里面输入`find`+`文件名`
![图片来至github](http://7sbxbm.com1.z0.glb.clouddn.com/softmutate1.png)
#### 4.运算
必须先安装`sympy`,然后直接在对话框输入运算公式。
```
sudo apt-get install python-pip
sudo pip install sympy
```
`
![图片来至github](http://7sbxbm.com1.z0.glb.clouddn.com/softmutate2.png)
#### 5.杀死进程
在对话框里面输入`kill`+`进程名`，输入进程名过程会有提示。
![图片来至github](http://7sbxbm.com1.z0.glb.clouddn.com/softmutate3.png)
#### 6.Google翻译
前提是你得能翻墙，才用得了google的服务。翻墙详见我的[博客](http://hxong.com/tags/%E7%BF%BB%E5%A2%99/)
选中你要翻译的单词或者句子，然后按`Ctrl+T`就能翻译。或者在mutate对话框里面输入`tr`+`单词或句子`。
![图片来至github](http://7sbxbm.com1.z0.glb.clouddn.com/softmutate4.png)
![图片来至github](http://7sbxbm.com1.z0.glb.clouddn.com/softmutate5.png)
#### 7.快速搜索
在对话框输入`搜索引擎`+`内容`,例如：`baidu`+`mutate`,`google`+`mutate`,`github`+`mutate`
![图片来至github](http://7sbxbm.com1.z0.glb.clouddn.com/softmutate6.png)
![图片来至github](http://7sbxbm.com1.z0.glb.clouddn.com/softmutate7.png)
#### 8.打开网页
在对话框输入网址，例如`hxong.com`
![图片来至github](http://7sbxbm.com1.z0.glb.clouddn.com/softmutate8.png)
#### 9.关机、重启、注销
在对话框输入相应命令，如`shut down`,`log out`,`reboot`
我今天安装的这个版本有个bug，shutdown不是关机，而是重启。修复方法如下：
编辑`~/.config/Mutate/scripts/shutdown/shutdown.sh`这个文件把里面的`gnome-session-quit --reboot`改成`gnome-session-quit --power-off`
![图片来至github](http://7sbxbm.com1.z0.glb.clouddn.com/softmutate9.png)
#### 10.配置
在对话框输入`preference`,打开配置界面。
![图片来至github](http://7sbxbm.com1.z0.glb.clouddn.com/softmutate10.png)

