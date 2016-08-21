title: ubuntu 与windows双系统记录
date: 2016-02-27 13:11:06
tags: Others
---
### 安装
+ 直接参考 [链接](http://jingyan.baidu.com/article/60ccbceb18624464cab197ea.html)
### 问题
+ 有时会出现在进入ubuntu时只出现一横线，此时请重新设置一下easyBCD，把原有的ubuntu删除，然后再加入引导，boot区记得设置好，为大小在190M的的分区
+ 在windows进行硬盘分区后，可能会出现 
![问题](http://7xncgn.com1.z0.glb.clouddn.com/16-2-27/73553232.jpg) 
解决方式如下
    - 找出你的Linux盘在哪个分区以及grub目录位置，使用ls 测试
``` bash
grub rescue> ls     
grub rescue> ls (hd0,0)/grub    //循环使用该命令测试所有的盘，直至显示该分区所包含内容而不是“unknown filesystem"
```
    - 假设我们试到了(hd0,8),成功显示了内容
```
grub rescue> set                     //显示set的格式，重新设置的格式也要同这个显示的格式相同
grub rescue> set  root=(hd0,8)       //括号里为上一步尝试成功的分区
grub rescue> set  prefix=(hd0,8)/grub  
grub rescue> insmod /grub/normal.mod //有时的路径不一定就是grub/normal.mod，要看ls (hd0,8)/grub的显示结果,如果显示没有normal.mod的话，但是有i386-PC目录的话，那么就写为insmod /grub/i386-PC/normal.mod
```
    - 到这里应该会应该退出了grub rescue模式，进入了熟悉的grub模式
```
grub> normal
```
    - 进入linux系统，在终端里面
```
sudo update-grub 回车
sudo grub-install /dev/sda
```
