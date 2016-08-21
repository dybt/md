layout: post
date: 2013/09/10
title: 【LaTeX Tips】输出罗马数字
description: 本文解决如何在 LaTeX 中输出罗马数字的问题。
category: LaTeX
tags: [Roman, Number]
image:
  feature: so-simple-sample-image-1.jpg
  credit: Michael Rose
  creditlink: http://mademistakes.com
share: true
comments: true
---

{% code demo lang:tex %}
%罗马数字演示
\documentclass{ctexart}
\newcommand*{\rmn}[1]{\romannumeral#1}
\newcommand*{\RMN}[1]{\uppercase\expandafter{\romannumeral#1}}
\begin{document}
\RMN{2013}年\RMN{9}月\rmn{10}日
\end{document}
{% endcode %}

如此即可。

效果见[http://pan.baidu.com/share/link?shareid=4006105280&uk=1728178661](http://pan.baidu.com/share/link?shareid=4006105280&uk=1728178661).
