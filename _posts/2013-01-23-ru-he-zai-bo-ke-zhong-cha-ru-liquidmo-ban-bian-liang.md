---
layout: post
title: "如何在博客中插入Liquid模板变量"
date: 2013-01-23 22:07
comments: true
tags: Octopress
categories: Other
---

上周写了一篇博客《[让Octopress在新标签页中打开外链](/blog/2013/01/13/rang-octopresszai-xin-biao-qian-ye-zhong-da-kai-wai-lian/)》，里面嵌入了一段代码，但是里面有一个模板变量`{% raw %}{{ root_url }}{% endraw %}`却怎么也显示不出来~，后来在Octpress的Issues页面问了一下才知道如何才能不把那些用来显示的代码块中的变量不被解析，具体看[https://github.com/imathis/octopress/issues/941#issuecomment-12206038](https://github.com/imathis/octopress/issues/941#issuecomment-12206038)或者Stack Overflow的[How to escape liquid template tags?](http://stackoverflow.com/questions/3426182/how-to-escape-liquid-template-tags)