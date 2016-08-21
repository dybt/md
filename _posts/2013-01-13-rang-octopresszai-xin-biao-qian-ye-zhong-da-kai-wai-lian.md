---
layout: post
title: "让Octopress在新标签页中打开外链"
date: 2013-01-13 19:30
comments: true
tags: Octopress
categories: Other
---

今天发现Octopress一个小小的问题，就是无法将外链在新窗口中打开。于是就自己写了一小段javascript代码轻易的解决了这个问题。
<!--more-->
代码如下：
{% gist 4523641 open-in-blank.js %}

用法很简单：

1. 将上面的代码下载下来或者复制其内容；
2. 保存到octopress目录的source/javascripts/子目录中；
3. 修改source/_includes/custom/header.html，在下面插入如下代码即可：

{% codeblock source/_includes/custom/header.html lang:html %}
{% raw %}
<script type="text/javascript" src="{{ root_url }}/javascripts/open-in-blank.js"></script>
{% endraw %}
{% endcodeblock %}
