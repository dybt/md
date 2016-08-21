---
title: hexo 部署
date: 2016-04-07 22:58:05
tags:
---
想搭建一个个人博客，又没有主机服务器的屌丝来说，[github pages](https://pages.github.com/)是一个不错的选择。github推荐使用[Jekyll](https://jekyllrb.com/)，用ruby开发的一个框架，由于本人最近在使用nodejs，所以选择了[hexo](https://hexo.io/)，下文记录了hexo的安装过程。

### 安装nodejs
[nodejs](https://nodejs.org/en/)是一个基于Chrome JavaScript运行时建立的平台， 用于方便地搭建响应速度快、易于扩展的网络应用。从官网下载安装包，安装过程不再详细叙述。

### 安装hexo

``` bash
$ npm install-g hexo
```
{% img /img/20160407/1.png 1394 956  %}

### 部署hexo

新建一个文件夹blog，初始化应用。
``` bash
$ mkdir blog
$ cd blog
$ hexo init
```
{% img /img/20160407/2.png 1394 956  %}

###  生成静态页面
``` bash
$ hexo generate
```

{% img /img/20160407/3.png 1394 956  %}

### 启动本地服务，进行文章预览调试，命令：
``` bash
$ hexo server
$ hexo server --debug
```
{% img /img/20160407/4.png 1394 956  %}

效果如下：
{% img /img/20160407/5.png 1394 956  %}

### 配置github
#### 创建Repository
{% img /img/20160407/6.png 1394 956  %}

建立与你用户名对应的仓库，仓库名必须为【your_user_name.github.io】，这是固定写法。和本地建立关联。我本地的目录是blog，里面的文件有：_config.yml    node_modules    public      source    db.json        package.json    scaffolds  themes 。我们需要_config.yml文件，来建立关联。修改部分：

``` bash
deploy:
  type: git
  repository: git@github.com:hellowego/hellowego.github.io.git
  branch: master
```   

### 部署到github
``` bash
$ hexo deploy
```   

{% img /img/20160407/7.png 1394 956  %}

效果图访问页面：http://hellowego.github.io/
{% img /img/20160407/8.png 1394 956  %}

### 通过自己的域名访问
#### 查询hellowego.github.io的ip地址
{% img /img/20160407/9.png 1394 956  %}

#### 设置ip解析 
{% img /img/20160407/10.png 1394 956  %}

在浏览器中http://hellowego.com/
或者：
www.hellowego.com/
{% img /img/20160407/11.png 1394 956  %}






















