---
layout: post
title: "如何通过GitHub与Octopresss来搭建自己的博客"
date: 2013-01-05 21:05
comments: true
tags:
- GitHub
- Octopress
categories: Other
---

现在通过[GitHub][github]+[Octopress][octopress]来搭建个人博客的越来越多了。这几天我也试着通过这个试着搭建一下，开始觉得很折腾，后来折腾出来之后发现还是买简单的。

通过[GitHub][github]+[Octopress][octopress]搭建的博客有几个很显而易见的有点：
<!-- more -->
1. 简单，虽然一开始觉得还是很复杂的，但是等搭建好一次之后发现还是很简单的；
2. 完全免费，不需要考虑空间费用问题，甚至域名的费用也不用考虑；
3. 空间够大，如果只写文字类的博客来说[GitHub][github]提供1GB的空间，已经足够用了
4. 插件多，[Octopress][octopress]也有很多插件（包括[Jekyll][jekyll]的插件），虽然不能和Wordpress比，但是也够用了~。如果自己有能力的话也可以自己写一些
5. 有版本控制，随时恢复到以前任何一个版本
6. ……

废话不多说了，下面就介绍来一下博客的搭建过程。

## 一些必要元素

- [GitHub][github]账号
- Git环境
- Ruby环境（1.9.3）
- Markdown编辑器（用于写日志用）

### 注册GitHub账号
这个很简单，只需要到[GitHub][github]按照上面的提示注册就可以了

### 搭建Git环境
* 如果是Mac的话建议用[Git OS X Installer][git-osx-installer]
* 如果是Windows的话请下载[msysgit][msysgit]并安装
* 如果是Linux
	* Debian/Ubuntu：`sudo apt-get install git-core`
	* CentOS/Redhat/Fedora： `sudo yum install git-core`
	* 其他Linux系统可以到[Git官网](http://git-scm.com)(需要翻墙)或者[GitHub](https://github.com/git/git)下载源代码进行编译安装
	
装好之后需要对git进行简单的配置（如果是Windows需要打开Git Shell）
```sh
git config --global user.email "your-email@example.com"
git config --global user.name "Your Name Here"
```

然后生成证书（如果是Windows需要打开Git Shell）
```sh
ssh-keygen -t rsa -C "your-email@example.com or other message"
```

然后拷贝~/.ssh/id_rsa.pub（Windows一般在C:\Users\<username>\.ssh\id_rsa.pub）的内容，登陆[GitHub][github]，进入"Account Settings"，点击"SSH Keys"，再点"Add SSH Keys"，将id_rsa.pub的内容粘贴到"Key"中，并输入"Title"

### 安装Ruby
使用[Octopress][octopress]需要Ruby版本为1.9.3，如果小于这个版本则无法使用。下面介绍一下如何安装Ruby~

如果你用的操作系统是非Windows则比较简单，强烈建议安装[rbenv](https://github.com/sstephenson/rbenv)或者[RVM](https://rvm.io/)，通过它们来安装ruby1.9.3，这样不会对你系统本身造成任何影响。以个人经验在Mac下安装Ruby比较麻烦一些。如果你之前安装过XCode4.1（必须装了Command Line Tools）或者之前的版本是比较简单的，否则需要先安装`apple-gcc42`，`apple-gcc42`你可以通过[Homebrew](http://mxcl.github.com/homebrew/)（[源代码](https://github.com/mxcl/homebrew)）或者[OSX GCC Installer](http://kennethreitz.com/xcode-gcc-and-homebrew.html)（[源代码](https://github.com/kennethreitz/osx-gcc-installer)）来安装。

如果你用的是Windows的话需要通过[RubyInstaller][rubyinstaller]来安装安装Ruby，到上面的页面下载最新的Ruby和DevKit。首先安装Ruby（记得把Ruby加入到系统PATH变量，安装的时候有这个选项），然后双击解压下载的DevKit（自解压文件），通过命令提示符cd到解压的目录，执行`ruby dk.rb init`，然后再执行`ruby dk.rb install`，这样ruby就安装好了。

### Markdown编辑器
* Mac推荐用[Mou](http://mouapp.com/)，国人开发的，免费的，你可以向作者提供一些[赞助](http://mouapp.com/donate/)
* Windows可以用[MarkdownPad](http://markdownpad.com/)
* Linux就自己找吧，应该也比较多
* 除了以上介绍的几个编辑器之外，很多代码编辑器也支持

## 搭建你的Blog
### 配置与部署
配置[Octopress][octopress]其实还是蛮简单的，只要根据官网上的[文档](http://octopress.org/docs/)一步一步来就可以了。我就简要介绍一下步骤吧（Windows下要在Git Shell命令行中执行命令）~

首先要到[GitHub][github]上创建一个仓库，仓库的名字是`<username>.github.com`，其中`<username>`是你的github上的用户名。创建好之后需要进入刚才建立的仓库，然后进入"Settings"，往下滚你会看到"GitHub Pages"，里面有个"Automatic Page Generator"按钮，点击它。然后是生成的首页和内容，别管它只需要点下面的"Continue to Layouts"按钮就行。接着就是选择模板，随便选一个然后按"PUBLISH"按钮即可。大约十分钟之后就可以看到通过"Automatic Page Generator"生成的页面了，当然这个你不需要管它，过一会会被[Octopress][octopress]生成的静态页面给替换掉。

接下来就要把[Octopress][octopress]克隆下来了，在终端输入如下命令（可以参考[http://octopress.org/docs/setup/](http://octopress.org/docs/setup/)）：
```sh
cd /path/where/you/want/to/clone
git clone https://github.com/imathis/octopress.git
cd octopress
# 安装依赖
gem install bundler
bundle install
# 安装Octopress默认的主题
rake install
```

然后是进行部署配置，执行命令（参考网址[http://octopress.org/docs/deploying/github/](http://octopress.org/docs/deploying/github/)）：
```sh
rake setup_github_pages
```
它会询问你并让你输入刚才所创建的仓库的地址：`git@github.com:<username>/<username>.github.com`，接下来是要配置一下你的_config.yml文件（参考网址：[http://octopress.org/docs/configuring/](http://octopress.org/docs/configuring/)），其实也比较简单，根据自己的实际情况修改一下就行了，这里就不做过多的解释了。

然后你可执行一下命令来预览你的Blog：
```sh
rake generate # 生成文件
rake preview # 生成本地预览，地址是 http://localhost:400
```
如果你觉得我没有问题就可以通过下面的命令将博客部署到GitHub上了
```sh
rake deploy # 会将通过rake generate生成的文件_deploy下的内容push到GitHub的master分支上
```
记得输入如下命令将所有的改动push到GitHub上
```sh
git add .
git commit -m 'your message'
git push origin source
```
### 撰写Blog
在终端中执行如下命令：
```sh
rake new_post['Post Title']
```
此命令会在source/_post/目录下生成一个类似于2012-01-05-post-title.markdown的文件，用Markdown编辑器编辑这个文件，最上面的`---`中间的部分是yml格式的一些关于文章的信息，具体含义请参考：[https://github.com/mojombo/jekyll/wiki/yaml-front-matter](https://github.com/mojombo/jekyll/wiki/yaml-front-matter)

编辑好文章之后执行如下命令：
```sh
rake generate # 生成文件
rake preview # 如果你想在本地查看一下的话
rake deploy # 部署到GitHub上
```
然后你应该就可以在`http://<username>.github.com`上看到你写的文章了，更多命令请参考[http://octopress.org/docs/blogging/](http://octopress.org/docs/blogging/)

### 绑定自己的域名
如果你想绑定自己的域名也是可以的，方法也很简单~。首先要参考GitHub上的帮助文章[https://help.github.com/articles/setting-up-a-custom-domain-with-pages](https://help.github.com/articles/setting-up-a-custom-domain-with-pages)来设置自己域名的DNS。然后执行下面的命令（[参考](http://octopress.org/docs/deploying/github/#custom_domains)）：
```sh
echo 'yourdomain.com' > source/CNAME
```
接下来就是等待域名解析生效了~

### 一些小提示
记得每次写完文章之后用如下命令将写的文章同步到GitHub上：
```sh
git add -A
git commit 'your message'
git push
```
这样你的文章就不会丢失了~，哪怕是重装电脑~，下次只需要把GitHub仓库clone下来然后重做上面的某些步骤就可以了!不过你也可以将文件保存到[Dropbox](http://db.tt/062Uch29)上，也是很方便的。

更多信息和更高级的内容请参考Octopress的[官方文档](http://octopress.org/docs/)和Jekyll的[Wiki](https://github.com/mojombo/jekyll/wiki)


[github]: https://github.com/
[octopress]: http://octopress.org/
[jekyll]: http://jekyllrb.com/
[git-osx-installer]: http://code.google.com/p/git-osx-installer/downloads/list
[msysgit]: http://code.google.com/p/msysgit/downloads/list?q=full+installer+official+git
[rubyinstaller]: http://www.rubyinstaller.org/downloads/