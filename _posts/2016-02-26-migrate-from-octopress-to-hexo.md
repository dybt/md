---
title: 从 Octopress 迁移到 Hexo
date: 2016-02-26 14:16:24
tags:
- Octopress
- Hexo
categories: Other
---

很久没写过日志了，原因没有别的，实在是鄙人太！懒！了！自己也想不能继续懒下去，所以决定改变从这里开始，先换个静态博客生成系统吧。好吧~，换系统其实和这个没多大关系，主要是因为 Octopress 已经 N 久没有更新了，还有就是对于一个 N 久没有写过博客的人来说，再用 [Octopress] 需要重新熟悉一下，相比之下 [Hexo] 就简单多了，还有就是据说生成速度上 [Octopress] 要比 [Hexo] 慢一大截。下面介绍一下 Mac 下 Hexo 的安装过程吧~

<!-- more -->

### 安装 Node.js 与 Hexo

建议通过 [Homebrew](http://brew.sh) 安装，原因是安装、升级和删除简单方便。当然也可以通过官网的安装包，但是通过安装包安装会有个问题，由于 `/usr/local/lib/dtrees` 和 `/usr/local/lib/node_modules` 目录的属主是 root:wheel 而不是当前用户，所以在执行 `npm install hexo-cli -g` 的时候会无法写入 `/usr/local/lib/node_modules` 而导致安装失败。如果用了 `sudo` 会有本地没有安装 `hexo` 的错误，让执行 `npm install hexo --save` 命令，因此也无法使用。

通过上述方法安装完 [Node.js](https://nodejs.org) 之后执行 `npm install -g hexo-cil` 即可完成安装。

### 常用的命令

- `hexo init [blog directory name]` 初始化博客目录，如果后面目录名没有指定则是当前目录
- `hexo g[enerate]` 生成静态文件
- `hexo config` 获取和配置你的博客，可以通过 `_config.yml` 来直接更改配置（推荐！因为比较直观，但是要对 [Yaml] 格式有一定的了解，当然也是非常简单的）
- `hexo n[ew] <[post]|page> <title>` 创建文章或页面
- `hexo s[erver]` 建立并开启本地服务器，默认网址是 http://0.0.0.0:4000
- `hexo d[eploy]` 部署博客
- `hexo m[igrate]` 从其他平台迁移到 [Hexo]
- `hexo --help` 显示帮助，可以查看别的命令的帮助

### 配置

[Hexo][Hexo] 的配置也是很简单的，只需要简单配置如下选项即可（其他的根据需要自己选择性的改一改就行了）：

```yaml
# Site，网站基本信息相关的内容
title: Fire Studio
subtitle: 记录，学习，成长
description: iOS Developer
author: Yozone Wang
language: zh-Hans
timezone: Asia/Shanghai

# URL，网站网址
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: http://firestudio.cn

# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/ ，可以到这里查看和使用别的主题，
theme: next

# Deployment 部署，最主要的，但是也很简单
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: git@github.com:azone/azone.github.io.git
```

预览之后才发现 [Hexo] 不支持对 [Yaml] 格式代码的高亮。

### 迁移与部署

由于我之前用的是 [Octopress] 而且他们的格式是相互兼容的，所以最简单的办法是只需要把之前 [Octopress] 下 `_post` 目录拷贝到 [Hexo] 即可。但是有一点需要注意，一定要将之前的 `.markdown` 后缀改成 `.md`，否则文章生成的地址会是 `/yyyy/MM/dd/yyyy-MM-dd-title` 而不是 `/yyyy/MM/dd/title`

如果是通过 git 或者 Github 部署的，则需要通过`npm install hexo-deployer-git --save`先安装 hexo-deployer-git 插件。

当然也可以安装 [Hexo] 提供的 [插件](http://hexo.io/plugins) ，然后通过 `hexo m[igrate] <type>` 迁移，最后通过 `hexo d[eploy]` 来部署。

### 我安装的几个插件

由于今天才开始部署 [Hexo]，所以对 [Hexo] 的插件了解的也不是太多，只装了几个：

- [hexo-deployer-git](https://github.com/hexojs/hexo-deployer-git)，通过 [GIT](http://git-scm.com) 部署博客
- [hexo-generator-feed](https://github.com/hexojs/hexo-generator-feed)，生成订阅
- [hexo-generator-category](https://github.com/hexojs/hexo-generator-category)，生成分类
- [hexo-generator-sitemap](https://github.com/hexojs/hexo-generator-sitemap)，生成网站地图
- [hexo-ruby-character](https://github.com/JamesPan/hexo-ruby-character)，{% ruby 为了好玩  %}装了这个插件😄

### 总结

该说的上面已经说了，总的来说还是很棒的。最后决定把评论系统从 [Disqus](https://disqus.com) 迁移到[多说](http://duoshuo.com)，毕竟多说是国内的，速度应该会快很多，Disqus 说不定哪天就被墙了。由于评论内容前一起来非常麻烦，而且评论内容非常少，所以决定评论内容就不迁移了。不说了，走起~

---

*注意：文章中方括号中括起来的部分为可选部分*

[Octopress]: http://octopress.org
[Hexo]: https://hexo.io
[Yaml]: http://yaml.org
[Counter]: http://notes.xiamo.tk/2015-10-21-为NexT主题添加文章阅读量统计功能.html


