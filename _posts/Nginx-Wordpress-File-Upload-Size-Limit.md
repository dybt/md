---
title: 记一次修改Nginx服务器上Wordpress文件上传的大小限制
date: 2016-5-4 20:22:33
tags:
  - nginx
  - Wordpress
  - 文件上传限制
categories: 服务器
---

## 前言

关于这个问题也碰到过几次了，大多数需求是需要在`Wordpress`后台通过`HTTP`协议上传大的视频文件，有时会遇到因为所在的服务器限制了上传文件大小而无法上传较大的附件，`WordPress`的媒体文件上传可以看到，大多数都是`2MB`或者`8MB`。如果是图片的话可能会还够用，但是如果是音频和视频文件就不一定够用了，所以在此做一个记录。
服务器配置为：`Nginx+PHP5+MySQL`

<!-- more -->

## 查看PHP配置文件`php.ini`的路径

首先在网站根目录下建一个`info.php`文件，文件内容如下：

```php
<?php
echo phpinfo();
?>
```

然后在浏览器中访问这个文件，如：`http://localhost/info.php` 或者 `http://www.niices.com/info.php`
我们可以看到`php.ini`的位置，如图：
![](/images/2016/05/1.png)
则该服务器上`php.ini`的位置是：/etc/php5/fpm/php.ini

## 修改`php.ini`文件

编辑打开`php.ini`文件
搜索如下字段：
```
memory_limit
post_max_size
upload_max_filesize
max_execution_time
max_input_time
```

一般默认的设置值为：

```
memory_limit=128M　　　  //单个线程可调用内存大小
post_max_size=8M　　　　 //设定 POST 数据所允许的最大大小。一般要设置的比upload_max_filesize大
upload_max_filesize=2M　 //默认上传文件大小，这个就是2M的限制所在
max_execution_time=30　　//最大执行时间，页面等待时间，超过该时间脚本就会停止执行
max_input_time=60　　　　//限制通过POST、GET以及PUT方式接收数据的时间，一般设置为比max_execution_time大
```

然后将其改为自己需要的值，例如：

```
memory_limit=128M
post_max_size=1200M
upload_max_filesize=1024M　//这样就改为可以传1024M以下的文件了
max_execution_time=300
max_input_time=600
```

## 查看`nginx`配置文件`nginx.conf`的路径

当我们执行`nginx -t`的时候，`nginx`会去测试我们的配置文件语法是否正确，同时也就告诉了我们配置文件的路径：

```bash
$ nginx -t
```

结果如图：
![](/images/2016/05/2.png)
所以该服务器的`nginx`配置文件路径为`/etc/nginx/nginx.conf`

## 修改`nginx.conf`文件

打开`nginx.conf`并在`http{}`(或者`server{}`或者`location / {}`)字段里添加

```bash
http {
    ...
    client_max_body_size 1024M;
}
//或者
server {
    ...
    client_max_body_size 1024M;
}
//或者
location / {
    ...
    client_max_body_size 1024M;
}
```

## 重启服务使修改生效

重新加载`nginx`服务

```bash
$ /etc/init.d/nginx reload
```

重新加载之前可以先检查一下`nginx`配置文件是否正确

```
$ nginx -t
```
重启`php`服务

```bash
$ service php5-fpm restart
```

大功告成！