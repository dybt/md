title: 利用hexo搭建属于自己的静态博客
date: 2015-05-28 10:12:48
tags: [hexo,]
---
### 环境
<hr>
本文将介绍对`ubuntu`系统下的`Hexo 3.0`的安装过程
#### 什么是Hexo?
[Hexo][1] 是一个快速,简洁且高效的博客框架. Hexo 使用 `Markdown`(或其他渲染引擎)解析文章，在几秒内,即可利用靓丽的主题生成静态网页. [Hexo 文档][2] 
[1]: https://hexo.io "Hexo"
[2]: https://hexo.io/zh-cn/docs/ "Hexo 文档"
#### 安装前提
安装Hexo相当简单. 然而在安装前, 必须检查电脑中是否安装一下应用程序.
- Node.js
- Git

在`ubuntu`下直接安装`npm`和`git`就可以
``` bash
$ sudo apt-get install npm
```
安装npm会默认安装node.js
``` bash
$ sudo apt-get install git
```
安装后可以通过以下命令安装hexo
``` bash
$ npm install -g hexo-cli
$ npm install hexo --save
```
上面的命令在`win`下可以进行但是在ubuntu下可能会出现`/usr/bin/evn: node: No such file or directory`的错误,主要是因为`ubuntu`下已有叫`node`的库.`Node.js`在ubuntu下默认为`nodejs`，输入以下命令
``` bash
$ sudo ln -s /usr/bin/nodejs /usr/bin/node
```
这样就没什么问题了, 可以通过上述内容安装`hexo`
<!-- more -->
### Hexo 配置 
<hr>
#### 博客目录
安装完成后, 创建一个空的目录`blog`, 进如目录后初始化就能看到`hexo`的文件了
``` bash
$ mkdir blog
$ cd blog
$ hexo init
```
#### 安装Hexo 插件
``` bash 
npm install hexo-generator-index --save
npm install hexo-generator-archive --save
npm install hexo-generator-category --save
npm install hexo-generator-tag --save
npm install hexo-server --save
npm install hexo-deployer-git --save
npm install hexo-deployer-heroku --save
npm install hexo-deployer-rsync --save
npm install hexo-deployer-openshift --save
npm install hexo-renderer-marked@0.2 --save
npm install hexo-renderer-stylus@0.2 --save
npm install hexo-generator-feed@1 --save
npm install hexo-generator-sitemap@1 --save
```
完成后可通过`hexo server` 来访问`localhost:4000`查看博客
#### Hexo 配置文件
Hexo 通过`_config.yml`来配置静态博客

```base
    title: mink home  # 注意 `:` 后一定要有空格不然会出错, 不容易找出，建议统一
    subtitle: 记录属于自己的旅程. # 类似小标签
    author: mink # 作者信息
    language: zh-CN # 语言设置，不然默认是繁体

    url: http://cangmean.org
    ...

    per_page: 10 # 一页显示最多文章数

    theme: jacman   # 博客主题，我采用的是jacman
    stylus:
        compress: true

    # 把静态博客上传到 我的github page上
    deply:
        type: git  #
        repo: https://github.com/kmean/kmean.github.io.git
        branch: master
```

主题配置请跳转到[jacman 主题配置][3]
[3]: http://wuchong.me/blog/2014/11/20/how-to-use-jacman/ "jacman 主题配置"

### 写文章
Hexo 使用`markdown`语法的`.md`文件
生成文件 `name.md`
``` bash 
$ hexo new 'name'
```
可以`vim blog/source/_post/name.md` 查看文件

```base
    title: 文章标题 
    data: 日期
    categories: 分类
    tags:   文章标签 [python, java] 这样显示多标签
    ----
    以下是文章内容
```

hexo 提供一些简写的命令方式
``` bash
hexo n # new
hexo g # generate 用于生成pulibc目录
hexo s # server 启动服务
```

