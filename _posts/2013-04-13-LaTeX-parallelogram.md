layout: post
date: 2013/04/13
title: 【LaTeX Tips】绘制平行四边形符号
description: 在 LaTeX 中绘制平行四边形符号。
category: LaTeX
tags: [Parallelogram]
---
平行四边形符号不是国际标准符号，而是我国「自造」的一个符号，所以在LaTeX本身以及常用符号宏包中都是未定义的。最方便的方法就是「自造」一个符号，利用 Ti*k*Z 之类的绘图工具。以下是代码：

<!--more-->

{% code demo lang:tex %}
\documentclass{article}
\usepackage{amsmath}
\usepackage{tikz}
\newcommand\parallelogram{
\mathord{\text{
\tikz[baseline] \draw (0,.1ex) -- (.8em,.1ex) -- (1em,1.6ex) -- (.2em,1.6ex) -- cycle;}}}
\begin{document}
[\parallelogram ABCD]
[E_{\parallelogram ABCD}]
\end{document}
{% endcode %}

以下是效果：
![](http://ww1.sinaimg.cn/large/818901c1jw1e3cis9s1v9j.jpg)
