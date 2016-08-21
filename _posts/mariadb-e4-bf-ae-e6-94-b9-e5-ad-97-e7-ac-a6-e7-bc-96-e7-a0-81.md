---
title: Mariadb 修改字符编码
id: 122
categories:
  - 数据库
date: 2015-06-15 12:28:34
tags:
	- mysql
	- 数据库
---

<span style="font-size:20px;">&nbsp; &nbsp;最近在数据库的时候遇到了字符编码的问题,用了一点时间终于解决了这个问题,现在来分享一下解决的方法.</span>

	&nbsp;

	<span style="font-size:20px;">&nbsp; 首先,在mariadb中输入 show variables like &#39;character%&#39;,然后就会看到你的字符编码,结果如图:</span>[![2015-06-15 20:12:09 的截屏](http://www.dreamleilei.com/wordpress/wp-content/uploads/2015/06/2015-06-15-201209-的截屏-300x154.png)](http://www.dreamleilei.com/wordpress/wp-content/uploads/2015/06/2015-06-15-201209-的截屏.png)

	<span style="font-size:20px;">从图片中可以看到你的server字符编码是latinl的,而client的字符编码是utf8的,你的database的编码也是utf8的,为了正常显示中文,需要将你的server和database的编码都设置成utf8的,</span>

	<span style="font-size:20px;">vim打开/etc/my.cnf</span>

	[![2015-06-15 20:13:01 的截屏](http://www.dreamleilei.com/wordpress/wp-content/uploads/2015/06/2015-06-15-201301-的截屏-300x183.png)](http://www.dreamleilei.com/wordpress/wp-content/uploads/2015/06/2015-06-15-201301-的截屏.png)￼

	<span style="font-size:20px;">找到[mysqld] 和[mysql],连个模块,然后添加character_set_server=utf8 和 character-set = utf8 然后保存,重启一下数据库</span>

	<span style="font-size:20px;">mariadb的重启命令是systemctl restart maria.db然后登录mariadb&nbsp;<span style="line-height: 32px;">&nbsp;show variables like &#39;character%</span></span>[![2015-06-15 20:17:01 的截屏](http://www.dreamleilei.com/wordpress/wp-content/uploads/2015/06/2015-06-15-201701-的截屏-300x127.png)](http://www.dreamleilei.com/wordpress/wp-content/uploads/2015/06/2015-06-15-201701-的截屏.png)

	<span style="font-size:20px;">看到字符编码都统一了为utf8,这样就解决了字符编码的乱码问题.</span>

	&nbsp;

	<span style="font-size: 20px; line-height: 32px;">&nbsp; &nbsp;对于以前已经创建好的表,可以用alter table db.tb convert to character set &lt;字符编码&gt;来实现问题的解决.</span>

	&nbsp;

	&nbsp;