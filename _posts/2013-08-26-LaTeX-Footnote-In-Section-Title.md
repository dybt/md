layout: post
date: 2013/08/26
title: 【LaTeX Tips】在 Chapter/Section 等标题中插入脚注
description: 直接在章节标题中插入脚注会出现问题，本文提供了解决方案。
category: LaTeX
tags: [Footnote, Section, Chapter]
image:
  feature: so-simple-sample-image-5.jpg
  credit: Michael Rose
  creditlink: http://mademistakes.com
share: true
comments: true
---

{% code demo lang:tex %}
\documentclass{article}
\begin{document}
\tableofcontents
\section[Section Title]{Section Title\footnote{This is footnote.}}
\end{document}
{% endcode %}

如此即可。
