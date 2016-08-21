title:  Flex与Bison应用实例
date: 2016-02-28 09:17:31
tags: Project
---

### **Flex应用实例**
#### 要求与注意
+ 从标准输入中逐行读取待检验的字符串。使用正则表达式匹配 IPv4 地址,若匹配失败则输出 Invalid ,若匹配成功则输出 IPv4 地址的类别。
+ 从标准输入中读取 DOT文件,文件无词法、语法错误。
+ 根据 token.txt 使用正则表达式匹配 token 并且输出。
+ Flex 中必须使用 C 代码,无需自定义 main 、 yywarp 函数, gcc编译时添加 -lf 或者 -ll 选项即可自动链接这些函数。
+ DOT 关键词大小写不敏感。
+ DOT 只包含英文字母和符号,即在 ASCII 范围内。
+ DOT 的 ID 与 C 语言相同, STRING 里面可包含转义的双引号,即 \" 。
<!-- more -->

#### 实现
+ 相关实现代码和输入输出文件 [Github](https://github.com/sysuKinthon/Compile/tree/master/FLEX)
#### 参考链接
+ http://lesliezhu.github.io/public/flex.html

### **Bison 应用实例**
#### 要求与注意
+ 使用Flex、Bison解释正则表达式,以文本方式输出其语法结构
+ 例子 ![](http://7xncgn.com1.z0.glb.clouddn.com/16-2-28/49085827.jpg) 

#### 实现与体会
+ 实现代码 [Github](https://github.com/sysuKinthon/Compile/tree/master/Bison)
+ 体会
    - 这个主要是理解如何去构建一个没有二义性的产生式来解决正则的匹配问题，以下对产生式说明下:
    - ![例子1](http://7xncgn.com1.z0.glb.clouddn.com/16-2-28/82339094.jpg)

    - ![例子2](http://7xncgn.com1.z0.glb.clouddn.com/16-2-28/48634203.jpg)
    - 对于产生式的构建 
        * 一个规则单元尽量不要使用exp:exp exp的形式，而要使用左递归的形式，比如上图中的concat产生式；
        * 为了防止规约/规约冲突，将规则(不同的非终结符的语法规则)按照自上而下(规约的顶层为上)的顺序排列，上面的规则会代先，也就是说在我们的文法中我们就定义好我们的优先级。
        * 可以借鉴
        ![借鉴](http://7xncgn.com1.z0.glb.clouddn.com/16-2-28/36180940.jpg)
    - 我们以ab|cd这个输入来说明，其输出应该为Alt(Cat(Lit(a), Lit(b)), Cat(Lit(c), Lit(d))),是怎么运行的 首先yyparse调用yylex得到一个Lit类型的token,其匹配到了single:Lit这个产生式，产生的single又匹配了bracket:single,同理将会发生exp:bracket,concat:exp的匹配，到这里，发生的规约与移入冲突，也就是alt:concat要求规约，concat:concat exp要求移入(编译是向前看一步的，b可以规约到exp，所以提示要移入)，在Bison中在规约与移入冲突中的优先移入的；所以就会发生b的token移入，同时到达concat:concat exp后就会规约到一个concat,接下来再到alt:concat后，alt:alt "|" concat提示要移入。所以最后就会产生Alt(Cat(Lit(a), Lit(b)), Cat(Lit(c), Lit(d)))这样的一棵语法树(语法树的文本型需要根据自己构建的语法树来生成)

#### 参考资料
+ http://epaperpress.com/lexandyacc/