layout: post
date: 2013/12/04
title: 【LaTeX Tips】如何复写定理
description: 有时我们会需要在文本的同一行开始一个列表环境，这里是解决方法。
category: LaTeX
tags: [Theorem, Tecall, THM-RESTATE]
image:
  feature: ./LaTeX/recall-theorem.png
  credit: Liam Huang
  creditlink: http://liam0205.me/about/
share: true
comments: true
modified: 2013-12-05
---

在书写定理时，我们可能会将定理的结论写在前面，而后展开对它的讨论。定理复杂的证明过程，通常会放在后文的附录中。这时候，我们就需要复写这个定理。

下面这个来自 [TeX.SX](http://tex.stackexchange.com/a/51288/38350) 的方法由 [lockstep](http://tex.stackexchange.com/users/510/lockstep) 提供。

<!--more-->

{% code demo lang:tex %}
\documentclass{article}

\usepackage{thmtools}
\usepackage{thm-restate}

\usepackage{hyperref}

\usepackage{cleveref}

\declaretheorem[name=Theorem,numberwithin=section]{thm}

\begin{document}

\section{First}

\begin{restatable}[Goldbach's conjecture]{thm}{goldbach}
\label{thm:goldbach}
Every even integer greater than 2 can be expressed as the sum of two primes.
\end{restatable}

\section{Second}

We recall \cref{thm:goldbach}:

\goldbach*

\end{document}
{% endcode %}

`thmtools` 包里的 `thm-restate` 子包提供了 `restatable` 环境。下面的例子里，借用了 `hyperref` 宏包（超链接）和 `cleveref` 宏包（提供了 `\cref` 宏，可以加载正确的定理样式）。具体可以查看 `thmtools` 宏包文档的 1.4 节。

效果：

<figure>
    <a href="/images//LaTeX/recall-theorem.png"><img src="/images//LaTeX/recall-theorem.png"></a>
    <figcaption><a href="/images//LaTeX/recall-theorem.png" title="复写定理的效果展示">复写定理的效果展示</a></figcaption>
</figure>

-----------------------

We sometimes hope that we could show and discuss a theorem, without proof it. However, in appendix, we mey need to recall that theorem and then write down the details of proof.

The following context comes from [TeX.SX](http://tex.stackexchange.com/a/51288/38350) that is provided by [lockstep](http://tex.stackexchange.com/users/510/lockstep).

{% code demo lang:tex %}
\documentclass{article}

\usepackage{thmtools}
\usepackage{thm-restate}

\usepackage{hyperref}

\usepackage{cleveref}

\declaretheorem[name=Theorem,numberwithin=section]{thm}

\begin{document}

\section{First}

\begin{restatable}[Goldbach's conjecture]{thm}{goldbach}
\label{thm:goldbach}
Every even integer greater than 2 can be expressed as the sum of two primes.
\end{restatable}

\section{Second}

We recall \cref{thm:goldbach}:

\goldbach*

\end{document}
{% endcode %}

The `thm-restate` package which is part of `thmtools` offers a `restatable` environment. In the following example, I also use `hyperref` and `cleveref` (its `\cref` macro automatically adds the correct theorem type). See section 1.4 of the thmtools manual for details.

效果：

<figure>
    <a href="/images//LaTeX/recall-theorem.png"><img src="/images//LaTeX/recall-theorem.png"></a>
    <figcaption><a href="/images//LaTeX/recall-theorem.png" title="Recalling a theorem.">Recalling a theorem.</a></figcaption>
</figure>
