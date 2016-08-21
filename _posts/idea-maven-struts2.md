---
title: Idea + maven + struts2
id: 132
categories:
  - IDE 使用

date: 2015-06-19 12:34:18
tags: 
	- Intellij IDEA
---

<span style="font-size:20px;">今天下午花了两个 多小时才把maven +struts 2 + idea 的环境给搞定,正可谓是坑爹啊!</span>

	&nbsp;

	&nbsp; &nbsp;&nbsp;<span style="font-size:20px;">首先,还是之前一样,创建maven项目,然后选项一个webPP,创建完成后,在依赖管理中,添加struts2的依赖关系,</span>

	![2015-06-19 18:09:29 的截屏](http://www.dreamleilei.com/wordpress/wp-content/uploads/2015/06/2015-06-19-180929-的截屏-300x300.png)](http://www.dreamleilei.com/wordpress/wp-content/uploads/2015/06/2015-06-19-180929-的截屏.png)[![2015-06-19 18:09:54 的截屏](http://www.dreamleilei.com/wordpress/wp-content/uploads/2015/06/2015-06-19-180954-的截屏-283x300.png)](http://www.dreamleilei.com/wordpress/wp-content/uploads/2015/06/2015-06-19-180954-的截屏.png)

	&nbsp;

	<span style="font-size: 20px; line-height: 32px;">添加这两个依赖就好了,然后配置web.xml</span>

	[![2015-06-19 18:12:51 的截屏](http://www.dreamleilei.com/wordpress/wp-content/uploads/2015/06/2015-06-19-181251-的截屏-300x58.png)](http://www.dreamleilei.com/wordpress/wp-content/uploads/2015/06/2015-06-19-181251-的截屏.png)

	<span style="font-size:22px;"><span style="font-size:20px;">然后还要配置struts.</span></span><span style="font-size:22px;"><span style="font-size:20px;">xml 这个标记要放在resources下</span></span>

	&nbsp;

	![2015-06-19 20:32:32 的截屏](http://www.dreamleilei.com/wordpress/wp-content/uploads/2015/06/2015-06-19-203232-的截屏-300x204.png)(http://www.dreamleilei.com/wordpress/wp-content/uploads/2015/06/2015-06-19-203232-的截屏.png)

	&nbsp;

	<span style="font-size:20px;">这样就配置好了struts2的开发环境,(当时因为不知道struts2.xml,放置在哪个路径下,所以走了好多弯路).</span>

	&nbsp;

	<span style="font-size: 20px; line-height: 32px;">&nbsp; &nbsp; &nbsp; &nbsp;配置国际化的时候出现乱码的情况,首先,写好的相应的properties文件,然后在终端下用native2ascii -encoding UTF-8 input.properties output.properties 最终生成utf8编码的文件,然后还应该设置struts的国际化资源的编码,在struts.xml中配置如:</span>

<pre style="background-color:#2b2b2b;color:#a9b7c6;font-family:'Source Code Pro';font-size:16pt;">
<span style="color:#e8bf6a;">&lt;constant </span><span style="color:#bababa;">name</span><span style="color:#a5c261;">=&quot;struts.i18n.encoding&quot; </span><span style="color:#bababa;">value</span><span style="color:#a5c261;">=&quot;UTF-8&quot;</span><span style="color:#e8bf6a;">/&gt;</span></pre>

	<span style="font-size:20px;">然后在JSP的文件,设置JSP文件的编码格式,内容如下:</span>

	&nbsp;

<pre style="background-color:#2b2b2b;color:#a9b7c6;font-family:'Source Code Pro';font-size:16pt;">
&lt;%@ <span style="color:#cc7832;font-weight:bold;">page </span>contentType=&quot;<span style="color:#6a8759;">text/html;charset=UTF-8</span>&quot; language=&quot;<span style="color:#6a8759;">java</span>&quot; pageEncoding=&quot;<span style="color:#6a8759;">UTF-8</span>&quot; %&gt;</pre>

	<span style="font-size:20px;">这样就完成了,字符编码的统一.</span>