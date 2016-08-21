---
title: JSP复习
id: 195
categories:
  - Java复习
date: 2015-07-24 16:14:22
tags:
	- Java
	- JSP
---

# 
	<span style="font-family:arial,helvetica,sans-serif;">&nbsp;复习了JSP 的一些基础的东西,总结一下.</span>

	&nbsp;

	<span style="font-family:arial,helvetica,sans-serif;"><span style="font-size:20px;">首先jsp是一门后台语言,但是在web开发中,如果用jsp+servlet+javaBean开发的话,jsp作为mvc中的view层,主要用来表现的.</span></span>

	<span style="font-family:arial,helvetica,sans-serif;"><span style="font-size:20px;">jsp的工作原理,其实jsp的本质就是servlet,当用户第一次访问一个jsp页面的时候,web容器把jsp页面编译成相应的servlet进行响应.</span></span>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">jsp的四种基本语法:</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">(1)jsp注释</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">(2)jsp表达式的输出</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">(3)jsp声明</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">(4)jsp脚本</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">(1)jsp注释:</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">注释的格式&lt;%-- --%&gt; html的注释格式&lt;!-- --&gt;</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">(2)jsp表达式的输出</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">&lt;%=表达式%&gt;(注意后面没有分号);</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">(3)jsp声明</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">&lt;%!</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">%&gt;</span></font><font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">&nbsp;在此声明,多次请求一个jsp只会初始化一次声明.</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">(4)jsp脚本</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">&lt;% 脚本 %&gt;凡是在&lt;% %&gt;中的内容都是jsp脚本的内容,可以在jsp脚本中插入html代码</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">jsp的三个编译指令</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">(1)page指令 &lt;%@ page .... %&gt;</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">这是对整个页面的一个描述后面可以加language=&quot;&quot; , charset=&quot;&quot;, contenttype=&quot;&quot;, import=&quot;&quot;, isErrorPage=&quot;&quot;, isElIgnored=&quot;&quot;, extends=&quot;&quot;.</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">(2)taglib指令&lt;%@ taglib uri=&quot;&quot; prefix=&quot;&quot;%&gt;(后面提到的两个属性都是必需的)</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">(3)include指令&lt;%@ include file=&quot;&quot;(必需的,且只有一个) %&gt;</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">JSP的七个指令动作:</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">(1)jsp:forward 转发</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">里面只有一个page属性且是必需的.转发的时候相应的url是不变的,可以通过&lt;jsp:param name=&quot;&quot; value=&quot;&quot; &gt;传入参数,且转发的时候之前的请求属性等等是不会丢失的.</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">(2)jsp:include 用于动态引入一个JSP页面</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">也可以通过&lt;jsp:param&gt;设置参数</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">&lt;page=&quot;&quot;该属性是必需的&gt;</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">&lt;autoflash=&quot;&quot;&gt;设置是否自动刷新</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">(3)jsp:param 初始化参数</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">一般和jsp:forward和jsp:include混合使用</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">(4)jsp:plugin</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">(5)jsp:useBean 引入JavaBean实例</span></font>

	<span style="font-size:20px;">&lt;jsp:useBean id=&quot;&quot;(必需的)&nbsp;class=&quot;&#39; beanName=&quot;&quot; scope=&quot;&quot;(可选属性application,page,request,session)</span>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">(6)jsp:setProperty 设置javaBean实例中的属性</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">&lt;jsp:setProerty property = &quot;&quot;(必需的) name=&quot;&quot;(必需的) &nbsp;value=&quot;&quot;</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">(7)jsp:getProperty 获取javaBean实例中的属性</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">&lt;jsp:getProperty property =&quot;&quot; name=&quot;&quot;&nbsp;</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">jsp的9个内置对象.</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">(1)request</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">对应于Servlet中HttpRequest对象,</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">(2)response</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">对应于Servlet中HttpResponse对象</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">(3)session</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">对应与Servlet中HttpSession对象</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">(4)page</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">对应于pageContext实例.</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">(5)config</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">对应于ServletConfig对象</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">(6)application</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">ServletContext实例</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">对应于整个web应用</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">(7)out</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">(8)exception</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">(9)pageContext</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">对应于ServletContext对象.</span></font>

	&nbsp;

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">JSP的生命周期:</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">(1)解析阶段:Servlet容器解析jsp代码,如果有错误,就返回错误.</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">(2)翻译阶段:将JSP代码翻译成Servlet代码</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">(3)编译阶段:Servlet容器编译相应的Servlet源文件,生成Servlet类</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">(4)初始化阶段:</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">(5)运行阶段</span></font>

	<font face="arial, helvetica, sans-serif"><span style="font-size: 20px; line-height: 32px;">(6)销毁阶段</span></font>