layout: post
date: 2013/08/25
title: 【LaTeX Tips】画一条自动延伸到行尾的线
description: 在 LaTeX 的使用过程中，我们有时候需要画一条自动延伸到行尾的线。本文提供了这样的解决方案，并且这条线可以调整高度位置/粗细/颜色。
category: LaTeX
tags: [Rule]
image:
  feature: so-simple-sample-image-3.jpg
  credit: Michael Rose
  creditlink: http://mademistakes.com
share: true
comments: true
---

LaTeX 向来是不擅长处理动态问题的，而我们的需求显然需要定义一个动态的长度。为了解决这个矛盾，我们可以退而求其次，选择用很短的一小节 rule 拼接在一起填充满整一行，而不是定义一个动态的长度。

示例代码在[这里](/attachment/RuleTillEnd/RuleTillEnd.tex).

理解思路之后，读者还可以定义出许多其他的「动态」，愿诸君都能勤思考。
