---
title: Hexo部署个人博客到Github Page和Coding Page并绑定域名
date: 2016-3-27 20:22:33
tags:
  - Hexo
  - Github Page
  - Coding Page
  - 个人博客
  - 自定义域名
categories: 个人博客
---
## 前言
为什么要在`Github`上面写博客？具体原因我就不阐述了。以前是用的`Wordpress`，也自己弄过一个主题，可是总是感觉不怎么满意。后来看到了`Hexo`，这种简单自由的写作方式吸引了我，于是就把自己的博客搬到了这里。折腾完了也该写个总结，本文主要介绍使用`Hexo`在`Github Page`和`Coding Page`搭建自己的免费静态`Blog`并绑定自己的域名，主题是使用的为什么要同时用`Github`和`Coding`两个服务呢？因为`Github`主动屏蔽了百度的爬虫，如果你希望你写的网站数据能够被百度抓取到，就只能用这种折中的方法，国内流量的解析到`Coding`，国外的解析到`Github`(这里可以用`DNSPod`的免费服务)。

<!-- more -->

## 准备
>本文主要针对`Windows`平台以及`Hexo 3.x`

### 关于Hexo
>A fast, simple & powerful blog framework

Hexo 是一个快速、简洁且高效的博客框架。`Hexo` 使用 `Markdown`（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。
官网地址：[hexo.io](https://hexo.io/)

### Node环境安装
Hexo是一个基于Node.js静态博客框架，所以我们还需要安装Node环境。
官网地址：[Node](https://nodejs.org/en/)

### Git安装
为了把我们的静态文件部署到`Github`和`Coding`上面，我们还需要`Git`这个版本控制工具，本文不讨论`Git`的详细使用方法，先挖个坑[Git简明使用方法]()。
官网地址：[Git](https://git-scm.com/download/)

### 安装Hexo
在电脑里面给`Hexo`安个家，我是安在`D:\Github\Hexo`文件夹下的，然后打开`Git Shell`，切换到工作目录，使用`npm`命令安装（`npm`的全称是Node Package Manager，是一个NodeJS包管理和分发工具）。
```js
npm install hexo-cli -g
npm install hexo --save

#如果命令无法运行，可以尝试更换[taobao的npm源](https://segmentfault.com/a/1190000000471219)
npm install -g cnpm --registry=https://registry.npm.taobao.org
```
## 本地配置
### 部署Hexo
在`Hexo`目录下运行以下命令，程序会自动安装所有需要的文件。
```js
hexo init
```
### 安装Hexo插件
一些必要的插件和部署到git所需的插件以及自动生成`sitemap`,`rss`的插件，建议安装。
```js
npm install hexo-generator-index --save
npm install hexo-generator-archive --save
npm install hexo-generator-category --save
npm install hexo-generator-tag --save
npm install hexo-server --save
npm install hexo-deployer-git --save
npm install hexo-renderer-ejs --save
npm install hexo-renderer-marked@0.2 --save
npm install hexo-renderer-stylus@0.2 --save
npm install hexo-generator-feed@1 --save
npm install hexo-generator-sitemap@1 --save
```
### 启动本地服务
执行以下命令，成功后在浏览器打开[localhost:4000](http://localhost:4000)可以查看效果
```js
hexo server
```
>All Done! 本地配置就这么简单。

## 部署静态网页到Github和Coding
### 注册设置GitHub
1.注册[Github](https://github.com)，自定义用户名如`niices`，则你的`Github`访问地址为[github.com/niices](https://github.com/niices)
2.创建仓库，在主页右上角头像左边有个加号，点击创建`New repository`。
![](/images/2016/03/2.png)
3.初始化仓库，`Repository name`前面必须和用户名一致如`niices.github.io`（以`github.io`结尾的仓库每个账号只能创建一个）。然后就可以访问`Github`分配给你的域名[niices.github.io](http://niices.github.io/)了
![](/images/2016/03/3.png)
>`Description`就是这个仓库的描述如`Personal Blog`，类型选择`Public`，这样别人才能访问你的网站，而且免费账户也不能创建私有仓库，这样才符合`Github`开源的思想。

### 注册设置Coding
1.注册[Coding](https://coding.net),自定义用户名如`niices`，则你的`Coding`访问地址为[coding.net/u/niices](https://coding.net/u/niices)
2.创建项目，项目名最好和用户名一致，这样才能通过[niices.coding.me](http://niices.coding.me)直接访问你的网站。
![](/images/2016/03/4.png)
>类型一定要选择公开，不然不能开启`Pages`服务，别人更不能访问。

3.新建`Coding-Pages`分支
`coding`的`pages`服务需要新建一个名为`coding-pages`的特殊分支，并设为默认分支，如下图所示：
![](/images/2016/03/5.png)
4.开启`coding-page`服务并绑定自定义域名，如下图：
![](/images/2016/03/6.png)
### 修改`_config.yml`文件
在`Hexo`根目录下找到`_config.yml`文件，进入编辑。找到`deploy`这项，修改为
```yml
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
- type: git
  repo: git@github.com:niices/niices.github.io.git
  branch: master
- type: git
  repo: git@git.coding.net:niices/niices.git
  branch: coding-pages

```
### 发布到Github和Coding服务器上面
1.新建文章，执行`new`命令，生成指定名称的文章至`hexo\source_posts\postName.md`。
```js
hexo new [layout] "postName" #新建文章
```
>其中layout是可选参数，默认值为post。也可以直接在`hexo\source_posts`新建`markdown`文件，但是要注意添加头部信息

```yml
---
title: 基于Hexo部署个人博客到Github Page和Coding Page并绑定自定义域名
date: 2016-3-27 20:22:33
tags:
  - Hexo
  - Github Page
  - Coding Page
  - 个人博客
  - 自定义域名
categories: 个人博客
---
```
2.执行`clean`命令，清除`database`和`public`问价夹下面的文件（第一次可不清除）
```js
$ hexo clean
```
3.执行`generate`命令，生成静态站点文件于`public`文件夹下
```js
$ hexo generate
```
>3.X版本下此命令可用`hexo g`命令代替

4.执行`sever`命令，开启本地服务器
```js
$ hexo server
```
>同样可以用`hexo s`命令代替

5.本地显示正常就可以执行`deploy`命令发布到服务器上面了
```js
$ hexo deploy
```
>可以用`hexo d`命令代替

如果一切正常，现在分别访问[niices.github.io](http://niices.github.io/)和[niices.coding.me](http://niices.coding.me)就可以访问到你的个人博客了。如果你还想绑定个人域名，请继续往下面看 ^_^

## 个人域名绑定
现在要实现国内走coding，海外走github，只要去修改dns解析记录，添加两条CNAME指向上面两个域名就可以了，如图：
![](/images/2016/03/7.png)
注意后面有个点`.`
`Coding`由于在前面开启`pages`服务的时候就绑定了域名，现在就可以直接访问了。
而`Github`还需要上传一个`CNAME`文件到静态网页的根目录才行，我们可以把这个文件放在`Hexo\source`文件夹下面，`deploy`的时候会自动帮你上传到网站根目录。`CNAME`文件里只需要放你想绑定的域名就可以了，如图：
![](/images/2016/03/8.png)
>注意：`CNAME`文件是没有后缀的

## 一点小技巧（不时补充）
### 用Hexo做网站时图片文件的放置
这个问题一开始让我头疼了一会儿，放在七牛吧，既麻烦又不好管理，听说微博图床不错，但我用着总感觉怪怪的。想过用自己的空间搭一个`img.niices.com`，总感觉写个文章引个图片还要传来传去就蛋疼。
后来看了下Hexo的[官方文档](https://hexo.io/zh-cn/docs/asset-folders.html)。将`_config.myl`文件中的`post_asset_folder`选项设为`true`。然后在`source`文件夹下新建一个`images`文件夹，然后就可以直接用`markdown`语法直接引用图片文件了！
如图：
![](/images/2016/03/9.png)
MarkDown使用相对路径引用图片文件的语法：
```md
![](/images/2016/03/images.png)
```
### 推荐一个主题-[NexT](https://github.com/iissnan/hexo-theme-next)
[使用文档](http://theme-next.iissnan.com/)
