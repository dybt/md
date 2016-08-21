title: ubuntu 15.10双系统 安装
date: 2016-06-03 15:22:58
tags: Others
---
### 前篇
+ 系统安装请下载[文件](https://yunpan.cn/OcSiQkTVfWY6iA)（提取码：612d）
+ 如果安装完后重新进入系统出现了找不到引导而进入了grup rescue的话，[参考](http://sysukinthon.github.io/2016/02/27/Windows-Ubuntu/)

<!-- more -->
### 中篇
#### 联网
+ 安装完系统后，进行yah3c安装联网，[参考](http://forum.ubuntu.org.cn/viewtopic.php?t=366743)
    - 安装的时候有时候会因为网卡的问题而出现问题，因为yah3c里面默认使用etho的。此时iwconfig查看你的网卡信息，如果没有eth0的话，就进入/etc/yah3c.config进行网卡配置，主要是将eth0改为你的电脑网卡

#### 整体环境
+ sudo apt-get update
+ sudo apt-get upgrade

#### 输入法
+ 15.10里面已经安装了fcitx，所以比较方便，如果需要安装五笔的话，直接 `sudo apt-get install fcitx-table-wubi`重启后有效

#### wifi
+ [参考](http://jingyan.baidu.com/article/03b2f78c5a3c975ea237ae09.html)

#### chrome安装
+ 在官网上下载相应的ubuntu版本，然后进行安装 `sudo dpkg -i ...deb`

#### wps for linux
+ [官网](http://linux.wps.cn/)下载 deb包
+ 安装完成后会无法启动程序，控制台会提示错误
```
error while loading shared libraries: libgthread-2.0.so.0: cannot open shared object file: No such file or directory  
```

+ 需要安装32位库
```
sudo apt-get install libfontconfig1:i386
sudo apt-get install libXrender1:i386  
sudo apt-get install libsm6:i386  
sudo apt-get install libfreetype6:i386  
sudo apt-get install libglib2.0-0:i386
```

#### QQ 安装
+ 安装wine
```
sudo apt-get install wine
```
+ 下载wineqq
    - 下载地址在[博客](http://ttop5.net/?p=1316)中查看
+ 安装
```
sudo dpkg -i WineQQ7.8-20151109-Longene.deb
```

#### 修改host访问google
+ host文件下载： https://github.com/yongbraver/SYSUv6-hosts
+ 修改/etc/hosts文件
+ 重新加载：sudo systemctl restart NetworkManager

#### 安装命令行字典
+ 安装sdcv
    - `sudo apt-get install sdcv`
+ 创建字典目录,存放字典
    - `sudo mkdir /usr/share/stardict`
    - `sudo mkdir /usr/share/stardict/dic`
+ 下载字典 [地址](http://abloz.com/huzheng/stardict-dic/zh_CN/)
+ 解压后放在/user/share/stardict/dic目录下，然后`sudo chmod -R 777 /user/share/stardict/dic`

#### 强化终端
+ 目标实现终端分屏，安装的工作是tmux
+ 为也使其终端支持颜色进行配置，新建一个.tmux.conf
```
set -g default-terminal "screen-256color"
```

+ 相关命令（先打入Ctrl+b后再输入其他命令，以此来区分是tmux的命令还是shell的命令）
```
Ctrl+b % 新建一个窗口
Ctrl+b o 切换窗口
```

