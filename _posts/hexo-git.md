title: 用github同步本地与vps上的hexo博客
date: 2016-02-14 17:50:03
categories: 生活
tags:
- hexo
- git
---
![](http://7sbxbm.com1.z0.glb.clouddn.com/hexo-githexo-git-00.png)
*用git来同步编辑vps上面的hexo博客*
<!-- more -->
<!-- toc -->
我的博客在美帝的搬瓦工vps上，以前都是用ssh连接主机，然后用vim编辑博客。搬瓦工的连接速度不给力，写博客的时候很卡，有时还卡死。所以我就想在本地的电脑上写博客，写好同步到github上，再用ssh登陆到vps，将github上的文件同步到vps上。如果你有更好的办法，望告知。本地电脑安装是ubuntu系统。
如果你对git没啥了解，请先学习[廖雪峰老师的git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)。我也是刚刚在学习的时候，想到要这么干，边学边用。
### 1、创建git仓库
登陆[github](https://github.com)，创建自己的博客git仓库。
![](http://7sbxbm.com1.z0.glb.clouddn.com/hexo-githexo-git-01.png)
例如我的[博客](http://hxong.com)hxong。描述自己想写啥写啥。
![](http://7sbxbm.com1.z0.glb.clouddn.com/hexo-githexo-git-02.png)
### 2、将原有vps上的博客同步到github上
ssh登陆vps，切换到存放博客文件(.md)的目录下，例如`hexo/source/_posts`。执行下面命令：
操作前记得备份`_posts`目录。
```shell
git init	#初始化git
git config --global user.name "hlx70375" 	#引号里面改成自己github上注册的用户名。
git config --global user.email "hlx70375@gmail.com"	#引号里面改成自己github上注册的邮箱。
git add *.md
git commit -m "first git vps to github" #引号里面的说明自己写
git remote add origin https://github.com/hlx70375/hxong.git #记得替换称你的git仓库名
git push -u origin master
```
然后输入用户名和密码，就同步到github上了。
![](http://7sbxbm.com1.z0.glb.clouddn.com/hexo-githexo-git-03.png)
### 3、将github的博客同步到本地
把原来所以博客都git到本地方便做一些修改编辑的管理。

```shell
cd ~ #切换到用户目录下
mkdir git 
cd git
git clone git@github.com:hlx70375/hxong.git #github同步到本地电脑，相应的改成自己名称
```
![](http://7sbxbm.com1.z0.glb.clouddn.com/hexo-githexo-git-04.png)
### 4、本地写博客同步到github
在`～/git/hxong/`目录写博客，然后git到github上。例如我现在写的这篇【用github同步本地与vps上的hexo博客】。
```shell
cd ~/git/hxong/
vim hexo-git.md
```
![](http://7sbxbm.com1.z0.glb.clouddn.com/hexo-githexo-git-05.png)
写好保存退出，然后git到github.com上。
```shell
git add hexo-git.md
git commit -m "add hexo-git.md|用github同步本地与vps上的hexo博客"
git push -u origin master
```
同步好，刷新github就可以看到如下：
![](http://7sbxbm.com1.z0.glb.clouddn.com/hexo-githexo-git-06.png)
### 5、将写好的博客从github同步到vps上
```shell
cd hxong/source/_posts/ #切换到博客文件目录下
git pull #同步
hexo generate #发布
```
![](http://7sbxbm.com1.z0.glb.clouddn.com/hexo-githexo-git-07.png)
刷新自己的博客就可以看到新博客发布了。
![](http://7sbxbm.com1.z0.glb.clouddn.com/hexo-githexo-git-08.png)
