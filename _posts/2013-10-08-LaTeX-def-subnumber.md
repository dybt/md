layout: post
date: 2013/10/08
title: 【LaTeX Tips】在 LaTeX 中实现「子定理」
description:
category: LaTeX
tags: [Subnumber, Definition]
image:
  feature: so-simple-sample-image-4.jpg
  credit: Michael Rose
  creditlink: http://mademistakes.com
share: true
comments: true
---
> 问：写了7个definition后，这里第8个definition是分开定义的，即编号为8.1,8.2，8.3 。。。怎么样才能做到？

>    \newtheorem{myDef}{Definition}


<!--more-->

答：
{% code demo lang:tex %}
\documentclass{article}

\usepackage{amsmath}
\newtheorem{myDef}{Definition}
\newtheorem{subDef}{Definition}[myDef]

\begin{document}
\begin{myDef}
content...
\end{myDef}

\stepcounter{myDef}
\begin{subDef}
content...
\end{subDef}
\begin{subDef}
content...
\end{subDef}

\begin{myDef}
content...
\end{myDef}
\end{document}
{% endcode %}
