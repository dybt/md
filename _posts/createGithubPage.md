---
title: github + hexo 搭建个人博客小结 
date: 2016-06-21 19:16:01
tags:
    - 博客
    - hexo
    - 折腾
categories:
    - 折腾
---


最近在用 github + hexo 搭建静态博客，记录下自己踩过的坑。
 1. 如何构建github Page 在此就不再啰嗦，网上都有不错的教程，可以参考[简书](http://www.jianshu.com/p/465830080ea9)
 2. hexo 是一款开源的**静态博客**生成器，用*node.js*语言开发 因此要想安装hexo，必须要安装node.js，本人用的是centos7，用
 
```shell
  sudo yum install nodejs
```
安装nodejs,安装完node.js之后也要安装*npm*工具，npm安装同样用
```shell
   sudo yum install npm
```
安装完成之后，需要用npm安装hexo，安装命令:
```shell
    npm
     install hexo -g
```
记得要-g参数，-g表示全局安装，不加-g表示非全局安装

## 如何写博客
先介绍几个简单的命令:
* **hexo clean** （清空hexo的public文件夹)
* **hexo generate** （第一次用的时候调用，表示生成静态文件)
* **hexo new [article]** (生成一片新文章）
* **hexo deploy** (提交文章）

调用**hexo new [article]** 命令的时候,会在source/_post/文件夹下生成article.md,我们编辑article.md就会生成一片新的文章啦

## 如何更换主题

如何更换主题呢，github上有好多开源的主题供大家选择啦，我当时是参考的知乎的[链接](https://www.zhihu.com/question/24422335)进行更改主题了，更改主题的时候要注意了，当你下载主题的时时候最好先fork到自己的github上，然后主题clone到你的静态博客根据经下的**themes**文件夹下，然后修改_config.yml文件，在主题的文件夹下有一个_config.yml文件，在自己的博客的亘路径下同样也有一份_config.yml文件，前者是对主题的配置文件，后者是对自己的blog的整体的配置文件，在修改主题的时候，要更改博客的_config.yml文件，关键字搜索theme，然后把后面的主题(默认是landscpace)修改为自己想要的主题，然后
```
hexo clean
hexo deploy
```
就可以完成主题的修改了，主题的内容也可以自己修改，在这里就不再陈述啦

## 安装插件
配置完自己的静态博客之后，需要配置插件，例如配置多说评论插件，站内搜索的等等，就不再啰嗦了

### 参考链接:
>[http://www.tuicool.com/articles/AfQnQjy/](http://www.tuicool.com/articles/AfQnQjy/)
> [http://www.jianshu.com/p/465830080ea9](http://www.jianshu.com/p/465830080ea9)
> [http://www.jianshu.com/p/739bf1305e66](http://www.jianshu.com/p/739bf1305e66)
> [http://www.isetsuna.com/hexo/install-config/](http://www.isetsuna.com/hexo/install-config/)
> [http://www.jianshu.com/p/619dab2d3c08](http://www.jianshu.com/p/619dab2d3c08)
