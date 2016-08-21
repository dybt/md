---
title: Mysql主从复制的实现
id: 241
categories:
  - 数据库
date: 2016-04-25 09:44:46
tags:
    - 数据库
    - mysql
---

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;最近想实现MySql的主从复制,在网上搜了资料后有两种实现方式,

&nbsp; &nbsp; &nbsp; &nbsp; (1)修改 /etc/my.cnf 文件,在从数据库的配置文件中修改

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;server-id=2

&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;host-name=&lt;hostname&gt;

&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;host-user=&lt;user&gt;

&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;host-password=&lt;password&gt;

&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;master-log-file=

&nbsp; &nbsp; 然后重启数据库

(2)在启动mysql后输入命令:

&nbsp; CHANGE MASTER TO

&nbsp; &nbsp; MASTER_NAME=&#39;&#39;,

&nbsp; &nbsp; MASTER_USER=&#39;&#39;,

&nbsp; &nbsp; MASTER_PASSWORD=&#39;&#39;,

&nbsp; &nbsp; MASTER_LOG_FILE=&#39;&#39;,

&nbsp; &nbsp; MASTER_LOG_POS&#39;&#39;;

START SLAVE;

后来在自己的mariadb 10.0下用第一种方式,发现mysql重启报错,后来查了一下资料是因为MySql5.5之后不再支持,所以用第二种实现

参考链接：http://blog.csdn.net/hw_libo/article/details/38072287

&nbsp;&nbsp;

&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;

&nbsp; &nbsp;