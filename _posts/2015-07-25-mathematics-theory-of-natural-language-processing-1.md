title: 自然语言处理的数学原理（一）
date: 2015-07-25 11:37:06
categories: Algorithm and Computer Science
tags: [Statistic Language Model, Natural Language Processing]
mathjax: true
---

一个基本的搜索引擎的工作，基本上可以分成以下三个部分：

1. 利用网络爬虫下载网页，分析网页关键词，制成索引备用；
2. 理解用户输入，确定检索关键词；
3. 根据关键词和网页索引，按照相关性排序列出搜索结果。

第一个部分主要涉及网络爬虫技术、图论、自然语言处理等技术；第二个部分主要涉及自然语言处理；第三个部分同样涉及自然语言处理。

> 自然语言，即是人类用来交流的语言。

由此可见，自然语言处理（NLP, Natural Language Processing）是现代搜索引擎很重要的内容，其终极目的是将自然语言转化为计算机容易处理的形式。

<!-- more -->

## 从分词的角度来看文法分析与统计模型

分词是 NLP 需要解决的基础问题，分词算法的好坏直接影响 NLP 的结果。

这里我们先从一个简单的例子说起，逐步探讨合理的分词算法。

### 从一个简单的句子说起

现在有一个句子，比如：

* 我去电脑城买了一台电脑。

如果要让计算机对这个句子做分词处理，进而理解这个句子，你会有怎样的思路呢？

大多数人首先会思考一下自己是怎么理解这个句子的。对于中国人来说，这样一个简单的句子，可能不需要什么特殊的思维过程。句子的文字形式和句子背后的含义可以在瞬间反映出来。稍有汉语文法知识的读者，可能会想：

1. 句子可以分成几个部分
    * 我 - 主语
    * 去电脑城买了一台电脑 - 谓语
        * 去电脑城 - 状语
        * 买了 - 谓语动词
        * 一台电脑 - 动词宾语（名词短语）
    * 。 - 句子结束的标识
2. 分别理解每个部分的意思
3. 将意思拼合起来，变成完整的句意

它先通过文法分析，将句子拆分成一个二维的语法树，然后再理解各个部分的含义，最后做拼接。

这样的方案（或者说是算法）是基于文法规则的，清晰明了，也易于实现（在计算机里就是几个循环判断）。对于程序员来说，这样的算法也特别亲切。因为程序员使用的高级编程语言（比如 C++）的语法规则和这样的方案非常相似。

由于这样的算法直观、易于实现，所以人们相信在有了愈加全面的文法概括和愈加强大的计算能力时，人们就能彻底解决自然语言处理的问题了。

### 文法分析的困境

然而，如果你仔细观察文法分析的过程就会发现，这么一个简单的句子被分成了一个这样复杂的二维树状结构，耗费了六条注释。用计算机来处理这样一个过程当然不难，但是要处理现实生活中遇到的真实句子，往往就不那么容易了：

* 由于理解（understanding）自然语言，需要关于外在世界的广泛知识以及运用操作这些知识的能力，自然语言认知，同时也被视为一个人工智能完备（AI-complete）的问题。

这个句子依然可以用上述方法来处理：

* 先分成主谓部分
* 再仔细拆分谓语部分

比如：

* 自然语言认知 - 主语 - 偏正短语
    * 自然语言 - 名词作定语修饰
    * 认知 - 名词
* 由于理解（understanding）自然语言，需要关于外在世界的广泛知识以及运用操作这些知识的能力 ... 同时也被视为一个人工智能完备（AI-complete）的问题 - 谓语
    * 由于理解（understanding）自然语言，需要关于外在世界的广泛知识以及运用操作这些知识的能力 - 原因状语
        * ...
        * ...
    * 同时也被视为 - 谓语动词短语
        * 同时 - 状语
        * 也被视为 - 谓语动词
    * 一个人工智能完备（AI-complete）的问题 - 动词宾语
        * 一个 - 定语
        * 人工智能完备的 - 定语
        * 问题 - 名词
    * 。 - 句子结束的标志

这个句子的语法分析树我没有写完，因为实在太复杂了。显而易见，单纯基于文法分析的分析器是很难处理生活中的真实句子的。

那么问题出在哪里？我认为至少有两个问题。

1. 文法规则数量巨大，上万条语法规则才只能覆盖约 20% 的真实句子；且有些为了处理特殊情况的语法规则和其他规则相互矛盾。
2. 自然语言与程序设计语言不同，自然语言中词汇的具体含义与上下文相关，而程序设计语言则没有这样的歧义性。

从算法复杂度的角度来说，单纯基于文法分析的分析器，用于分析自然语言，其复杂度比分析程序设计语言要高出四个量级。从直观的印象来说，上述句子在一台现代计算机上用文法分析的方式处理，也需要至少一分钟的时间。这种低效是无法接受的。

### 查字典分词法

在之前的文法分析方法里，分词依赖于文法分析的结果。程序要先输出语法树，然后才能得到分词结果。而这样的方法已经被证明是低效的。

这样的低效来源于复杂的文法分析过程。为了提高效率，人们很自然地想到：是否有办法绕开文法分析，直接尝试分词呢？对于中文分词，北京航空航天大学的梁南元教授提出了查字典分词法。做法相当简单，比如对于下列句子：

* 山东大学数学学院是中国最好的数学基础教育基地之一。

我们让计算机从左到右扫描整个句子，每扫到一个字，就往字典里查询，遇到字典里有的词就标注出来。于是整个句子就被分割成了这样：

* 山东|大学|数学|学院|是|中国|最好的|数学|基础|教育|基地|之一。

看起来结果不错。不过细心的读者很快就会发现：山东大学和基础教育都是完整的词，在它们之间不应该再做划分。会出现这种情况也不意外，我们要求计算机从左到右扫描。当计算机遇到「山东」二字的时候，就认为这是一个词了，自然不会再去寻找下一个字去寻求匹配。同理基础教育。

梁教授提出了一个方案，即总是搜寻尽可能长的分词。这在计算机科学领域叫做「贪婪」。运用贪婪的办法，上述句子的分词就会变成：

* 山东大学|数学|学院|是|中国|最好的|数学|基础教育|基地|之一。

看起来就没什么问题了。

不过，汉语博大精深，这种办法也不能一劳永逸。比如：

* 大学生活区

正确的分词应该是：

* 大学|生活区

但是按照贪婪的办法，会被分词成：

* 大学生|活|区

这就不对了。

又比如：

* 发展中国家

正确的分词应该是：

* 发展中|国家

而不是：

* 发展|中国|家

可见，查字典的办法虽然效率很高，但是时有出错，并不牢靠。

查字典的办法遇到的困境来自于自然语言的歧义性。人类在阅读自然语言时，会结合上下文判断有多个意向的词汇在文中的具体含义，但是计算机却没有这个能力。实际上，中国传统文学里说的「句读」，其目的就是通过分词断句来消除歧义。那么，怎么让计算机具备这样的能力呢？

### 千呼万唤始出来的统计模型

行文至此，数学终于要第一次展现其威力和美丽。

我们之前提到，对一个句子做分词，其正确与否和词汇的二义性紧密相关。由于计算机无力综合上下文判断词汇含义，解决二义性，所以查字典的办法陷入了困境。

数学中有所谓的「反证法」。在这里我们不讲反证法，但是要讲讲反证法的思想。反证法的核心思想就是「正难则反」：正面突破很困难，那就不走大路，开个后门照样进城。在这里，既然计算机没有能力综合上下文解决词汇的二义性，那么我们就不依赖计算机智能去解决，转而借助人工的力量解决。当然，我说的不是找一个工人实时干预程序的运行，帮助程序作出正确的判断；而是说，让计算机经过大量的文本训练，吸取人类的「分词经验」。而这个方法，就是统计模型。

假定一个句子 $S$ 可以有几种分词的方案，比如有以下三种：

\\begin{align}
    A_1,\,A_2,\,A_3,\,\ldots,\,A_j \label{eq:fenci-1}\\\\
    B_1,\,B_2,\,B_3,\,\ldots,\,B_k \\\\
    C_1,\,C_2,\,C_3,\,\ldots,\,C_l \\\\
\\end{align}

其中，$A_1$, $A_2$, $B_1$, $B_2$, $C_1$, $C_2$ 等都是汉语的词汇。这样一来，如果 \eqref{eq:fenci-1} 是最好的分词，那么 \eqref{eq:fenci-1} 出现的概率应该最大。也就是说，分词方案 \eqref{eq:fenci-1} 应该满足 \eqref{eq:stat-req}。

\\begin{equation}
\label{eq:stat-req}
\\begin{cases}
    P(A_1,\,A_2,\,A_3,\,\ldots,\,A_j) \gt P(B_1,\,B_2,\,B_3,\,\ldots,\,B_k) \\\\
    P(A_1,\,A_2,\,A_3,\,\ldots,\,A_j) \gt P(C_1,\,C_2,\,C_3,\,\ldots,\,C_l) \\\\
\\end{cases}
\\end{equation}

答案就是这么简单。

当然，如何处理 \eqref{eq:stat-req} 需要一点统计知识和技巧；得到这些分词方案也需要依靠动态规划算法（不然计算量太大）；还有诸如分词颗粒大小之类的细节问题需要处理。这些内容我们放在后续的小节里讨论，在这里，读者只需要知道这种利用统计的方法处理分词效果好、效率高就可以了。

### 小结

对于分词来说，统计模型的方法效率比文法分析的方法高，同时效果也要好。这里效率的提升是十分显著的。

此外，我们发现一个优秀算法背后的数学模型是十分简洁优美的。统计模型只需要一个概率不等式组就可以描述，而文法分析模型几乎无法构建一个可读的数学模型。我们在设计算法的时候，要尽可能追求简洁优美的数学模型，从简单粗暴做起，逐步完善完美。正如牛顿爵士所言「真理在形式上总是简单的，而不是复杂含混的」。

最后，文法分析方法是十分容易想到的，十分自然的处理方法，然而这种「自然」也使人误入歧途。这提醒我们，不可固执，不可迷信经验。