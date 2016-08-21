---
title: hexo+github博客的多机使用
date: 2016-07-31 21:22:46
tags:
    -blog
    -折腾
categories:
    -折腾
    -博客搭建
---

### 折腾

 &nbsp;&nbsp;&nbsp;&nbsp;暑期来到腾讯实习，公司给配了一台windows的电脑，因为之前一直用Linu,而且一直用linux搭建hexo博客来写文章，现在大部分的时间会用到windows系统，这个时候就需要搬迁自己的博客，也就是说能够让自己的博客在linux下和windows下工作。


 &nbsp;&nbsp;&nbsp;&nbsp;之前的想法一直很简单，以为只要简单的git clone下来就可以了，后来发现git clone下来的东西只有打包好的东西，也就是说一些源码和运行环境是没有的，而如果重新[安装heox博客](http://blog.dreamleilei.com/2016/06/21/createGithubPage/)的话一些配置参数都会设置为初始化，后来经过查找资料，找到以下解决方案。
	

首先在linux本地本地创建一个分支，命名为source 
  
```bash
git checkout -b source
```

在分支里把应该添加的东西都给添加进去。（public文件夹下的东西就不用再添加了，以后为这个是自动生成的，主要是posts文件夹下的东西，还有一些配置文件啥的。

添加远程分支。 

```bash
git remote add source url
```

然后把远程添加的远程分支add，commit然后push上去
这里push的时候记得要

```bash
git push origin source
```

这个时候就在linux分支上把应该做的工作都完成了。
接下来换windows系统。

```bash
git clone <url>
```

然后

```bash
git checkout -b source
```

```bash
git fetch origin source
```

如果在合并过程中出错的话可以强制远程覆盖本地

```bash
git fetch -all
```

然后

```bash
git reset --hard origin/source
```

最后

```bash
git fetch
```

以后每次操作前都应该

```bash
git fetch
```
然后写完后

```bash
hexo clean
hexo deploy
```

然后切换到source分支

```bash
git add
```

``` bash
git commit
```
 
```bash
git push
```

```bash
origin source 
```

这样就把master分支作为博客的呈现分支，而source分支作为博客的源码分支，以后换PC的时候只需要把source分支复制本地，就不会造成文章的丢失。



 
