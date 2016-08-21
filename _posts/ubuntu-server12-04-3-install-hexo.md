title: ubuntu server12.04.3安装hexo
date: 2016-01-13 06:28:48
categories: 生活
tags:
- hexo
---
![](http://7sbxbm.com1.z0.glb.clouddn.com/hexo.png)
*在ubuntu server 12.04.3上安装hexo*
<!-- more -->
详尽的教程请阅读[官方文档](https://hexo.io/zh-cn/docs/)
目录
<!-- toc -->
#### 1.安装Node.js
Hexo官方教程上面推荐的安装时方法是使用nvm

	wget -qO- https://raw.github.com/creationix/nvm/master/install.sh | sh
安装后，重启终端并执行下列命令即可安装Node.js。

	nvm install 4
#### 2.安装Hexo
上面的步骤都顺利的话，执行下列命令即可安装Hexo。

	npm install -g hexo-cli
#### 3.建站
[如果你没安装git，推荐你安装git，步骤如下]

	sudo apt-get install git
执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件。
*<folder>换成你要建站的目录*
```
hexo init <folder>
cd <folder>
npm install
```
我在本地电脑上安装的时候`hexo init <folder>`执行了好久，下载速度实在太慢了。
安装完，运行`hexo server`命令，就能运行hexo了。在浏览器上输入服务器地址+端口[hexo默认端口是4000]，如我的`192.168.1.5:4000`,就可以看到下面的界面了。
![hexo初见](http://7sbxbm.com1.z0.glb.clouddn.com/hexo1.png)
#### 4.nginx反向代理
我想正常没人喜欢在输入网址后面还要加上端口号。我们可以用ngnix将默认的80端口指向hexo的public目录[执行`hexo generate`才能生成public目录]。这样我们直接输入网址即可跳转到我们的hexo博客下面。这种方法可以不用在服务器运行`hexo server`命令，只要每次编辑完博客，运行下`hexo generate`接可以更新了。首先先安装ngigx：
	sudo apt-get install nginx -y
安装完成后编辑`/etc/nginx/sites-available/default`，编辑前先备份下原来的默认配置，这是个好习惯，万一被你折腾坏了，还有个默认配置可以恢复。
```
sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/default.bak
sudo gedit /etc/nginx/sites-available/default
```
将default里面的内容删除，复制粘贴下满内容:
```
server {
      listen 80;
      root /data/hexo/public;
      index index.html index.htm index.nginx-debian.html;
      server_name i.dental www.i.dental;
      location / {
          root /data/hexo/public;
      }
  }
```
`root`后面参数为你博客存目的目录记得是hexo下的public而不是直接hexo
`server_name`是你网站的域名，如果你没有域名，这行直接注释`#`。
如果你的电脑上放有几个网站，那么在default里面多写上几个server即可。
保存退出后，重启nginx：

	sudo /etc/init.d/ngigx restart
这时在浏览器打开你的服务器地址，应该可以看到：
![](http://7sbxbm.com1.z0.glb.clouddn.com/hexo2.png)
#### 5.写博客
##### a.编辑博客
普通的编辑器就可以编辑了，例如`gedit`和`vim`。我自己是用vim编辑的。
如果你不懂markdown语法，你最好自己去学习下markdown语法。hexo博客是用markdown语法来写的。
博客的源文件`*.md`放在博客目录下的`source/_post/`里面。下面我编辑下博客默认生成的`helle-world.md`文件。

	 vim /data/hexo/source/_posts/hello-world.md
##### b.新建博客
在博客的安装目录先执行`hexo new "newblog"`,即可生成一篇名为*newblog*的博客，相应的`source/_post/`目录下也会生成一个`newblog.md`文件。编辑就用上面的方法了。
