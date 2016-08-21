layout: post
date: 2013/11/05
title: 【LaTeX Tips】指定 include 的路径
description: 有时我们会将所有 sections 放在特定的路径下，再用 include 调用。为了不每次都输入这个路径，本文提供了一个解决办法。
category: LaTeX
tags: [Path, Include, Input]
image:
  feature: so-simple-sample-image-2.jpg
  credit: Michael Rose
  creditlink: http://mademistakes.com
share: true
comments: true
modified: 2013-12-02
---

## 问题的描述

在排版很长的文章时，为了便于组织结构，我们通常会将文章的正文部分分章节保存在其它的 `.tex` 文件中，最后在主控文档中使用 `\input` 或者 `\include` 命令来加载这些文档（参见[`\input` 和 `\include` 的区别](http://tex.stackexchange.com/questions/246/when-should-i-use-input-vs-include)）。

如果我们将这些章节源文件保存在指定目录下，比如 `./body/`, 那么实际使用时，我们必须在每一次调用 `\input` 或者 `\include` 时都输入这一目录。这无疑是麻烦的。我们希望找到一种解决方法，可以免去输入路径。

<!--more-->

## 解决方法

参考 [qingkuan 的回答](http://bbs.ctex.org/forum.php?mod=redirect&goto=findpost&ptid=77319&pid=454202&fromuid=109739), 我们可以在导言区添加如下内容以达到目的：

```tex
\makeatletter
\providecommand*\input@path{}
\newcommand\addinputpath[1]{
  \expandafter\def\expandafter\input@path
  \expandafter{\input@path{#1}}}
\addinputpath{body/}
\makeatother
```
