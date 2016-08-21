layout: post
date: 2013/10/11
title: 【LaTeX Tips】在 LaTeX 中输出多国语言
description: 以输出俄语为例，介绍如何在 LaTeX 中输出多国语言。
category: LaTeX
tags: [Multi-lingual]
image:
  feature: so-simple-sample-image-1.jpg
  credit: Michael Rose
  creditlink: http://mademistakes.com
share: true
comments: true
---

在 LaTeX 中输出英文之外的其他字符总是不那么自然，需要一些折腾。例如让 LaTeX 输出中文就走过了天元、CCT、CJK、xeCJK 这些路子。希望在 LaTeX 中输出俄文，可以参考一下来自[CTeX 论坛提问版](http://bbs.ctex.org/forum.php?mod=redirect&goto=findpost&ptid=77068&pid=453126&fromuid=109739)的方法。

<!--more-->

现代的方法是直接输入，使用 Unicode 编码。当然，俄国人自己用的话，可能会用他们的本地编码（CP1251 之类）。

需要做的有两点，一是使用直接支持 Unicode 的新编译命令（XeLaTeX 或 LuaLaTeX），或者使用让旧编译方式能处理 Unicode 的宏包（`inputenc` 加 `utf8` 选项）；二是选定包含俄文的字体。

需要与其他语言混合使用的时候，前一种方式会方便一些。

前一种方式像这样：
{% code demo lang:tex %}
% !TeX program = XeLaTeX
% !TeX encoding = UTF-8
\documentclass{article}
\usepackage{fontspec}
\setmainfont{CMU Serif}

\begin{document}

\section{Русский язык}
Русский язык

\end{document}
{% endcode %}

后一种方式像这样：
{% code demo lang:tex %}
% !TeX program = pdfLaTeX
% !TeX encoding = UTF-8
\documentclass{article}
\usepackage[utf8]{inputenc}
\usepackage[T2A]{fontenc}

\begin{document}

\section{Русский язык}
Русский язык

\end{document}
{% endcode %}
