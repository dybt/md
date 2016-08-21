layout: post
date: 2013/10/18
title: 【LaTeX Tips】LaTeX 公式上下花括号的交错
description: overbrace 和 underbrace 可以嵌套但是不能交错，本文解决了这个问题。
category: LaTeX
tags: [Overbrace, Underbrace, Rlap, Phantom]
image:
  feature: so-simple-sample-image-2.jpg
  credit: Michael Rose
  creditlink: http://mademistakes.com
share: true
comments: true
---

我们知道，LaTeX 数学模式下，为式子的上下添加花括号可以用 `\overbrace` 和 `underbrace` 这两个命令。他们可以互相嵌套，但是不能交错使用。这一特性有时候会给我们带来麻烦。

![](/attachment/images/brace-overlap.jpg)

为了排版如上图所示的效果，我们需要用 `\rlap` 重叠，同时用 `\phantom` 占位，见下面的代码。

<!--more-->

{% code demo lang:tex %}
\documentclass{article}
\usepackage{amsmath}
\begin{document}
\[
	\rlap{$\overbrace{\phantom{a \to b}}$} a \to \underbrace{b \to c}
\]
\end{document}
{% endcode %}
