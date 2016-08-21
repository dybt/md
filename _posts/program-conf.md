title: 一些常用的配置设置
date: 2015-12-31 03:13:20
categories: 编程
tags:
- 配置
- config
---
![](http://7sbxbm.com1.z0.glb.clouddn.com/programcode.jpg)
*我自己一些常用配置的备份，大家无视。*
<!-- more -->
### 目录
<!-- toc -->
### 1.终端配置
```
前景色：#708284
背景色：#07242E
字体:ubuntu mono | hack
```
下载：[ubuntu font](http://7sbxbm.com1.z0.glb.clouddn.com/programubuntu-font-family-0.83.zip)|[hack font](https://github.com/chrissimpkins/Hack)

### 2.lubuntu快捷键
配置文件位置~/.config/openbox/lubuntu-rc.xml
``` xml
<!-- 启动firefox -->
<keybind key="W-f">
  <action name="Execute">
    <command>firefox</command>
  </action>
</keybind>
<!-- 启动shadowsocks -->
<keybind key="W-f">
<keybind key="W-s">
  <action name="Execute">
    <command>sslocal -c .shadowsocks.json</command>
  </action>
</keybind> 
<!-- 启动aria2c -->
<keybind key="W-a">
  <action name="Execute">
    <command>aria2c --conf-path=.aria2.conf</command>
  </action>
</keybind>
<!-- 启动截图【区域截图，用鼠标直接选择，屏幕没有提示，保持在主目录下】 -->
<keybind key="C-A-a">
  <action name="Execute">
    <command>scrot -s</command>
  </action>
</keybind>
<!-- 启动ssh -->
<keybind key="W-p">
  <action name="Execute">
    <command>lxterminal -e "ssh -l [username] -p [port] [ip]</command>
  </action>
</keybind>
```

### 3.ubuntu快捷键
![](http://7sbxbm.com1.z0.glb.clouddn.com/programubuntukjj.png)
### 4.ubuntu主题、图标、壁纸下载
[DeviantArt](http://www.deviantart.com)
### 5.ubuntu字体
[方正兰亭刊黑](http://7sbxbm.com1.z0.glb.clouddn.com/fzltkh.tar.gz)
### 6.ubuntu使用gnome-shell
#### a.安装
	sudo apt-get install gnome-shell gnome-shell-extension gnome-tweak-tool
安装完注销，点登陆输入密码框的右上角的图标，可以选择进入gnome还是unity。
#### b.配置
打开`gnome-tweak-tool`，在扩展里面把`User themes`开启才能安装gnome-shell主题。我的配置:
```
主题:White
扩展:Applications menu,MMod panel,Topicons,User themes
```
![](http://7sbxbm.com1.z0.glb.clouddn.com/programgnome-shell_extension.png)
推荐：图标[vibrancy colos](http://7sbxbm.com1.z0.glb.clouddn.com/programvibrancy-colors_2.6~wily~Noobslab.com_all.deb),主题[xenlism](http://7sbxbm.com1.z0.glb.clouddn.com/programXenlism-Minimalism.tar.gz)|[arc](https://github.com/horst3180/arc-theme)。
图标双击安装即可，主题解压出来把整个目录复制到到`/usr/share/themes/`下。
![](http://7sbxbm.com1.z0.glb.clouddn.com/programxenlism.png)

