---
title: mysql的简单语句
id: 129
categories:
  - 数据库
date: 2015-06-15 14:28:18
tags:
	- 数据库
	- mysql
---

<span style="font-size:20px;">&nbsp; &nbsp; &nbsp; &nbsp; 最近在做一个小型的图书管理系统,用到了mysql的一点操作,不用不知道,一用吓一跳,码了一小会sql的语句,才发现自己的sql学的是如此的差,连基本的语句都不会了,更不用说是设计数据库了.</span>

	&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="font-size:20px;">基本的操作语句就不再陈述了,简单的,一些技巧性的东西,在此罗列一下,比如说想要让某一列的数值自动的增长,可以在建表的时候给某个列设置auto_increment 属性,然后设置在建表后的括号后面加上语句auto_increment=n ,n表示增长的基数. 然后移动字段,比如说你想把一个叫做Id的列移动到首行,这时候你要用到alter table &lt;表名&gt;&lt;列名&gt;&lt;属性&gt; first; 或者你想把某一个列移动到某个列的后面,这时候要用alter tabke&lt;表名&gt;&lt;列名&gt;&lt;属性&gt; after &lt;列名&gt;. 如果想要更改某个表的字符编码可以用alte table &lt;表名&gt; convert to character set &lt;字符编码&gt;.</span>

	&nbsp;

	<span style="font-size:20px;">&nbsp;&nbsp; &nbsp; &nbsp; 接下来,谈谈用户的增加删除,修改密码.</span>

	<span style="font-size:20px;">&nbsp; &nbsp; &nbsp;增加用户: 可以用create user &#39;&lt;username&gt;&#39;@&#39;&lt;host&gt;&#39; identitied by &#39;password&#39;&nbsp;</span>

	<span style="font-size:20px;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; 或者是insert into mysql.user(Host, User, Password) valuse(&#39;%&#39;, &#39;test&#39;, password(&#39;test&#39;)) &#39;%&#39;表示在任意主机可以登录,上条语句表示创建了一个test用户,密码为test. 要使命令生效,必须调用flush privileges.</span>

	<span style="font-size:20px;">&nbsp;&nbsp; &nbsp;删除用户:和增加用户相对立,drop user &#39;&lt;usernaem&gt;&#39;@&#39;&lt;host&gt;&#39; ;</span>

	<span style="font-size:20px;">&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;或者是通过修改mysql.user表来进行删除修改 delete from user where User = &#39;user&#39; and &nbsp;Host = &#39;host&#39;;</span>

	&nbsp;&nbsp;

	<span style="font-size: 20px; line-height: 32px;">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;给用户授权: grant &lt;privileges&gt; on db.tb to &#39;user&#39;@&#39;host&#39;其中db.db表示给用户某个表授权,如果是*.*表示对所有的数据库授权,db.*表示对db数据库里面的所有表授权,然后&lt;privileges&gt; 有select , update, delete , alter, index, 等权限.</span>

	<span style="font-size: 20px; line-height: 32px;">&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; 看一下某一个用户有哪些权限可以通过 show grants for &#39;user&#39;@&#39;host&#39;查看&nbsp;</span>

	<span style="font-size: 20px; line-height: 32px;">&nbsp;&nbsp; &nbsp; &nbsp; &nbsp;给用户删除权利: revoke&lt;privileges&gt; on db.tb from &#39;user&#39;@&#39;host&#39; 其中,所有的参数与授权的时候的参数是一致的.</span>

	<span style="font-size: 20px; line-height: 32px;">&nbsp;&nbsp; &nbsp;这个时候注意的是,如果你在授权的时候 grant all on db.special to &#39;lei&#39;@&#39;localhost&#39; 和 grant all on db.* to &#39;lei&#39;@&#39;localhost&#39;, 但是再给用户删除权利的时候只是调用revoke all on db.* 这个时候用户仍然具有 db.special的所有权限，这是一个比较特别的地方，具体的原因我也不知道．</span>

	<span style="font-size: 20px; line-height: 32px;">　　　用户密码的修改：可以通过　set password for &#39;user&#39;@&#39;host&#39; = password(&#39;newpassword&#39;)来进行修改,也可以通过修改 mysql.user表来进行修改.</span>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="font-size: 20px;">&nbsp;&nbsp;&nbsp;&nbsp;修改root密码:(1)在mysql.cnf中加上skip-grant-table然后重启mysql的服务,然后通过修改mysql的user表中的部分来进行密码的修改
</span>

<span style="font-size: 20px;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(2)mysqladmin -u root -p password newpasswd 然后输入密码,这个时候要用root权限,输入新密码
</span>

<span style="font-size: 20px;">
	&nbsp;</span>

	&nbsp;

	&nbsp;