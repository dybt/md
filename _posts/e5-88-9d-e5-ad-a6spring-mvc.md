---
title: 初学Spring MVC
id: 246
categories:
  - Spring MVC
date: 2016-04-27 13:06:34
tags: spring MVC
---

学习Spring MVC,于是想着用Spring MVC框架构建REST风格的项目,由于初次接触这个框架,所以犯了好多低级的错误,在此列出踩过的坑,防止以后再次入坑.

(1)在配置Spring MVC的xml文件中xsi:schemaLocation是键值对的关系,由于想要简化配置用到了context标签

&lt;context:component-scan base-package=&quot;&quot; &gt; 如果在xsi:scehmaLocation中没有配置context的url,会报错:通配符的匹配很全面, 但无法找到元素 &#39;context:component-scan&#39; 的声明 ,同理,其它的标签也是一样.

(2)在Controller中@RequestMapping(value=&quot;&quot;)愚蠢的写成了@RequestMapping(name=&quot;&quot;)于是报错了..

(3)在构建REST风格的应用的时候,因为自己用的是spring4.x,所以org.springframework.http.converter.json.MappingJacksonHttpMessageConverter是没有的,取而代之的是org.springframework.http.converter.json.MappingJackson2HttpMessageConverter

(4)在配置REST风格的应用的时候,jackson-databind是不可缺少的