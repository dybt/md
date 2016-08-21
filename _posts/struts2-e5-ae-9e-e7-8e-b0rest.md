---
title: struts2实现rest
id: 223
categories:
  - struts2
date: 2015-12-09 07:11:24
tags:
    - struts2
    - Rest
---

<span style="font-size: 20px;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp; 这学期跟着学长做了一个基于rest架构的php应用,学习了rest架构,便想着可以把之前自己做过的</span>[<span style="font-size: 20px;">软酷网</span>](http://korent.dreamleilei.com)<span style="font-size: 20px;">重构,把后天重构成rest格式的,试着用了一下strus2的rest插件.
</span>

<span style="font-size: 20px;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;首先说明一下什么是rest.&nbsp;</span>[<span style="font-size: 20px;">rest介绍</span>](http://www.ruanyifeng.com/blog/2011/09/restful.html)<span style="font-size: 20px;"> 个人理解rest 是一种思想,通过实现这种思想进行web端的架构,可以真正的实现前后端完全的分离,实现前端不必理会后台用什么语言实现,只需要知道相应的接口即可,而后台更不用管视图页面使如何展现的,只需要按照相应的规则,返回数据即可.
</span>

<span style="font-size: 20px;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;重构的想法是想把前端完全不用jsp.都用html,前端通过ajax请求来获取相应的数据,后端对所有的请求统一返回json.
</span>

<span style="font-size: 20px;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;而用户的登录想用web token来实现(暂时没有思路),但是用php的laravel框架做过相应的例子(JWAuth)
</span>

<span style="font-size: 20px;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;最初的网站用struts2作为controller层,实现rest架构,引入struts2-rest 和struts2-convention两个jar包,然后在struts.xml中进行 配置即可,进行相应配置的博文,网上搜一下一大堆,就不再墨迹,在这里罗列一下遇到的问题和要注意的事项.</span>

<span style="font-size: 20px;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 首先:版本的问题,因为struts2的版本太低,低于了struts2的2.1.16版本,结果在RequestUtil的类中找不到getUri方法,导致了无法正常web应用,后来花了好久的时间才找到原因,原来getUri方法,是2.1.16之后新增的方法,将struts2的版本设置成高版本之后,web应用可以正常运行.
</span>

<span style="font-size: 20px;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在struts2的约定中默认只会搜索后缀为Action的类作为Action,这个时候可以修改struts2.convention.suffix的值来指定Action的后缀,还可以通过制定struts2.convention.action.packages这个类来指定除了默认的包搜索Action类以外还要在哪些包下搜寻(记得要从根路径下开始写起),这个要和struts2.convention.action.pacakge.locators区别开来前者制定的是除了后者之外还要在的包下搜寻.
</span>

<span style="font-size: 20px;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;当strus2-rest和struts2-browser包用的时候,struts2-browser就会失效,这个时候,在struts.xml中制定</span>
<pre style="background-color:#2b2b2b;color:#a9b7c6;font-family:&#39;Source Code Pro&#39;;font-size:16pt;"><span style="font-size: 20px;"><span style="font-size: 20px; color: rgb(232, 191, 106);">&lt;constant </span><span style="font-size: 20px; color: rgb(186, 186, 186);">name</span><span style="font-size: 20px; color: rgb(165, 194, 97);">=&quot;struts.mapper.class&quot; </span><span style="font-size: 20px; color: rgb(186, 186, 186);">value</span><span style="font-size: 20px; color: rgb(165, 194, 97);">=&quot;org.apache.struts2.dispatcher.mapper.PrefixBasedActionMapper&quot; </span><span style="font-size: 20px; color: rgb(232, 191, 106);">/&gt;
&lt;constant </span><span style="font-size: 20px; color: rgb(186, 186, 186);">name</span><span style="font-size: 20px; color: rgb(165, 194, 97);">=&quot;struts.mapper.prefixMapping&quot; </span><span style="font-size: 20px; color: rgb(186, 186, 186);">value</span><span style="font-size: 20px; color: rgb(165, 194, 97);">=&quot;/rest:rest,:struts&quot; </span><span style="font-size: 20px; color: rgb(232, 191, 106);">/&gt;</span></span></pre>

<span style="font-size: 20px;">即可让config-browser生效.</span>

<span style="font-size: 20px;">上面两句话的意思是使用访问空间映射,所有命名空间为/rest的都用rest格式映射,所有根命名空间用struts映射.</span>

<span style="font-size: 20px;">&nbsp;&nbsp; &nbsp; struts-rest 不支持原生的put和delete请求,网上有相应的解决办法</span>

<span style="font-size: 20px;">&nbsp;&nbsp;&nbsp;&nbsp;参考链接:&nbsp;[http://www.cnblogs.com/yjmyzz/p/struts2-rest-plugin-advanced-usage.html](http://www.cnblogs.com/yjmyzz/p/struts2-rest-plugin-advanced-usage.html)
</span>