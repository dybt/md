---
title: Servlet复习
id: 190
categories:
  - Java复习
date: 2015-07-24 01:42:27
tags:
	- Java
	- Servlet
---

<span style="font-size:20px;">&nbsp;

	之前学过jsp,servlet和struts2基础,因为学完后不久就在准备最后的Java比赛和期末考试,好久没有写关于这些方面的代码了,于是趁着最近的时间复习一下有关这方面的内容.</span>

	<span style="font-size:20px;">我学的都是最基础的,最简单的东西.</span>

	<span style="font-size:20px;">首先谈一下webapp的工作路径吧,首先要有一个根目录,然后根目录下面有一个WEB-INF文件夹和src文件夹,在这个文件夹里面有一个web.xml文件(Servlet3.0以后这个文件可以没有),然后相应的jsp页面应该放在根目录下面,如果放在WEB-INF文件夹下面,是没有办法直接在浏览器里输入URL访问的,但是可以通过相应的映射访问,(有些页面出于安全考虑,可以放在WEB-INF下面,这样一定程度上可以提高安全性),一般WEB-INF文件夹下面有class文件夹和lib文件夹,其中lib文件夹是存放应用中需要用到的第三方jar包,class文件夹存放.class文件.

	根目录下的src文件夹是存放各种用到的源文件,比如自己写的Servlet文件或者Filter文件或者JavaBean.</span>

	<span style="font-size:20px;">接下来谈一下web.xml的配置,有一篇博客写的很棒,在此把它给粘上http://www.cnblogs.com/chinafine/archive/2010/09/02/1815980.html之前一直一位web.xml中的元素是没有顺序的,后来发现自己错的相当严重,导致花了好几个小时找错误,找不到哪里错了.

	最基础的是&lt;servlet&gt;元素一定在&lt;servlet-mapping&gt;元素之后,而&lt;context&gt;元素在&lt;servlet&gt;元素之前,&lt;context-param&gt;在&lt;servlet&gt;元素之前,在此列出一下最常用的一些元素的顺序:&lt;display-name&gt; , &lt;description&gt;, &lt;filter&gt;,&lt;filter-mapping&gt;, &lt;listener&gt;,&lt;servlet&gt; ,&lt;servlet-mapping&gt;,除了通过配置web.xml可以实现配置外,还可以通过注解的形式配置.</span>

	<span style="font-size:20px;">&lt;servlet&gt;配置:需要的子元素&lt;servlet-name&gt; &lt;servlet-classs&gt;(这两个元素是必须的)

	&nbsp;&lt;servlet-mapping&gt;配置:需要子元素&lt;servlet-name&gt;(此元素的值必须与&lt;servlet&gt;中的某个值匹配) &lt;url-pattern&gt;(次元素可以有多个值) (这两个元素是必须的)

	也可以通过注解的形式来进行配置Servlet(要求Servlet版本至少3.0)格式如下:

	@WebServlet(name=&quot;name&quot;, urlPatterns={&quot;pattern1&quot;, &quot;pattern2&#39;}, initParams={@WebInitParam(name=&quot;name&quot;, value=&quot;value&quot;), @WebInitParam(name=&quot;name1&quot;, value=&quot;value1&quot;)})</span>

	<span style="font-size:20px;">&lt;filter&gt;配置:和servlet的配置基本一致,在此就不再重复.</span>

	<span style="font-size:20px;">&lt;listener&gt;配置:只需要&lt; listener-class&gt;子元素</span>

	<span style="font-size:20px;">&lt;context-param&gt;配置:需要的子元素&lt;param-name&gt;和&lt;param-value&gt;(至少这两个元素,且只准出现一次)</span>

	<span style="font-size:20px;">说完了简单web.xml的配置,说一下简单的Servlet,创建一个Servlet必须要继承HttpServlet这个类,这个类中有好多方法,比如说do+请求方式方法,如doGet, doPost等等,这些方法的参数是HttpServletRequest类型和HttpServletResponse类型,如果是直接请求一个Servlet的话,是service()方法来对它进行响应的.</span>

	<span style="font-size:20px;">HttpServletRequest接口实现了ServletRequest接口,ServletRequest接口的方法有getContextType(), getContextLength(),getInputStream(), getLocalAddr(), getLocalName(), getLocalPort(),getParameter(String name),

	getAttribue(String name), setAttribute(String name, Object obj),removeAttribute(String name),getParameterNames(),getParameterMap()等等,HttpServletRequest接口中的方法:getCookies(), getHeader(String name), getHeaderNames(),getMethod(),getRequestURI(), getQueryString(),等.</span>

	<span style="font-size:20px;">HttpServletResponse接口实现了ServletResponse接口,ServletResponse接口的常用方法有:setContextType,setContextLength(),getOutputStream(),getWriter(),reset(),getCharacterEncoding(),flushBuffer(),

	HttpServletResponse接口中添加的方法有addHeader(String name, String value), setHeader(String name, String value), addCookie(Cookie cookie), setStatus(int sc), sendError(int sc);</span>

	<span style="font-size:20px;">ServletConfig接口中的方法getInitParameter(String name), getInitParameterNames(),getServleContext(),getServletName(),这个接口实例化后产生的是一个servlet的实例,而getInitParameter(String name)方法获取的参数来自于&lt;servlet&gt;里定义的或者在@WebServlet(initparameters)里面注解的参数.getServeletContext()方法返回一个ServletContext的实例,</span>

	<span style="font-size:20px;">ServletContext接口 ,每个Web应用有唯一的ServletContext对象,该接口中定义的方法有:setAttitute(String name, Object obj), getAttribuer(String name),getInitParameter(String name_, getInitParameterNames(), getAttributeNames(),getRequestDispatcher(String path), getRealPath(String path), log(String msg)</span>

	<span style="font-size:20px;">HttpSession接口 :getId(), invalidata(),setAttribute(String name, Object, obj), getAttribute(String name), getAttributeNames(), removeAttribueName(String name), isNew(),SetMaxInactiveInterval(int interval),getServletContext().</span>

	<span style="font-size:20px;">Cookie接口:</span>

	<span style="font-size:20px;">列举完了几个常用的Servlet接口,接下来谈一下Web应用,

	Web应用的生命周期:

	(1)启动阶段

	(2)运行阶段

	(3)终止阶段</span>

	<span style="font-size:20px;">Web应用的启动阶段:

	(1)将web.xml读入内存中

	(2)创建一个ServletContext实例

	(3)初始化Filter

	(4)初始化必要的Servlet(load-on-startup)</span>

	<span style="font-size:20px;">Web应用的运行阶段:Servlet都处于待命的状态</span>

	<span style="font-size:20px;">Web应用的终止阶段:

	(1)销毁Servlet实例

	(2)销毁Filter

	(3)销毁与JavaWeb应用有关的对象,如ServletContext</span>

	<span style="font-size:20px;">谈一下Servlet的生命周期:

	(1)初始化阶段

	(2)运行阶段

	(3)销毁阶段</span>

	<span style="font-size:20px;">初始化阶段:

	(1)加载.class文件到内存中

	(2)Servlet创建ServletConfig对象.

	(3)创建Servlet对象

	(4)调用Servlet的Init()方法</span>

	<span style="font-size:20px;">运行阶段:</span>

	<span style="font-size:20px;">销毁阶段;

	调用destory()方法.销毁Servlet对象,然后销毁ServletConfig对象.</span>

	<span style="font-size:20px;">包含与转发:

	ServletContext对象的getRequestDispatcher().forward() 或者.include();

	或者调用request对象的相应方法,两者的区别是前者只能使用绝对路径,后者可以使用相对路径.</span>

	<span style="font-size:20px;">重定向:

	response.sendRedirect(String location)重定向会丢失所有的请求参数.</span>

	&nbsp;

	&nbsp;