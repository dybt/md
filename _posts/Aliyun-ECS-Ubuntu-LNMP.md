---
title: 阿里云ECS Ubuntu系统下LNMP环境的搭建
date: 2016-3-19 20:10:33
tags:
  - 阿里云
  - Ubuntu
  - LNMP
categories: 服务器
---

## 前言

最近帮某公司做一个展示型网站，用的阿里云的`ECS`，虽然有各种`LNMP`一键安装包，但还是想自己跑一遍，顺便挖个坑记录一下以后配置服务器将会遇到的各种问题。言归正传，搭建了`LNMP（Linux+Nginx+MySQL+PHP）`网站服务器架构，服务器为阿里云服务器，系统为`ubuntu13.04` 64位。将要运行的应用为`Wordpress`网站程序。

备注：由于这几天一直有其他的事，当时没有写，也就没有截图，以后补上。

<!-- more -->

## 安装步骤

### 1.更新ubuntu的软件包安装源
``` bash
$ sudo apt-get update
```
### 2.安装nginx
``` bash
$ sudo apt-get install nginx
```
### 3.安装MySQL
``` bash
$ sudo apt-get install  mysql-server
```
>这里安装过程中会让你输入`MySQL`的`root`账户的密码（注意是`MySQL`的密码不是服务器的`root`密码）。

### 4.安装PHP
``` bash
$ sudo apt-get install php5 php5-fpm php5-cli php5-cgi php5-mysql php5-gd
```
>安装了一些`php`相关的组件。

## 配置软件

### 1.进入数据库
以`root`账户的权限进入数据库，输入密码
``` bash
$ mysql –u root –p
```
### 2.新建数据库
新建一个名为`wordpress`的数据库，并创建一个名为`wpuser`的用户，并将`wordpress`数据库授权给`wpuser`这个用户：
``` MySQL
mysql> create database wordpress;
mysql> grant all privileges on wordpress.* to wpuser@localhost identified by "password";
```
>这里的`password`用你自己的密码代替。

### 3.配置nginx
先检查一下`Nginx`有没有正确安装，启动`Nginx`：
``` bash
$ sudo service nginx start
```
浏览器打开`localhost:`端口号，如果出现Welcome to Nginx！等信息，说明一切正常，可以开始下一步了。
`nginx`的配置文件位于`/etc/nginx/`目录下，`nginx`的 配置文件是`/etc/nginx/nginx.conf`。打开默认配置文件：
我们可以看到`nginx.conf`文件的`http{}`模块里面有两行代码引用的外部文件:
``` bash
include /etc/nginx/conf.d/*.conf;
include /etc/nginx/sites-enabled/*;
```
这两行代码告诉我们`conf.d`文件夹下面所有以`.conf`结尾的文件和`sites-enabled`文件夹下面的所有文件都会被引用，所以我们可以把不同网站的配置文件放在这两个文件夹里面，而`sites-enabled`文件夹里面放的是`sites-available`文件夹下面的文件的软连接（相当于`Windows`里面的快捷方式），所以我们有三种方式来写我们网站的配置文件：
1>直接把配置文件写在`niginx.conf`文件的`http{}`模块下。
2>新建`site.conf`配置文件并将其放在`conf.d`文件夹下面。
3>进入`sites-available`文件夹，新建多个`site`文件（对应多个站点）放在`sites-available`文件夹下面，创建软连接链接到`sites-enabled`：
``` bash
$ cd /etc/nginx/sites-available
$ sudo vi wordpress
$ ln -s wordpress ../site-enabled/wordpress
```
### 4.配置nginx虚拟主机
用`vi`打开`nginx`默认配置文件
``` bash
$ sudo vi /etc/nginx/sites-available/default
```
`server`配置如下：
``` nginx
server {
        listen   80;
 
        root /var/www;
        index index.php index.html index.htm;
 
        server_name www.niices.com;
 
        location / {
                try_files $uri $uri/ /index.php?q=$uri&$args;
        }
 
        error_page 404 /404.html;
 
        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
              root /usr/share/nginx/www;
        }
 
        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        location ~ \.php$ {
                try_files $uri =404;
                #fastcgi_pass 127.0.0.1:9000;
                # With php5-fpm:
                fastcgi_pass unix:/var/run/php5-fpm.sock;
                fastcgi_index index.php;
                include fastcgi_params;
         }
}
```
检查你的`nginx`配置文件是否正确：
``` bash
$ nginx -t
```
如果打印出下面结果说明配置文件语法没有问题，具体业务需要自测：
![](/images/2016/03/1.png)
要让配置文件生效需要重新加载`nginx`服务:
``` bash
$ service nginx -s  reload
```
### 5.Wordpress安装
将`wordpress`解压到配置文件规定的`root`目录（这里是`/var/www`），进行安装，安装的详细过程就不说了！