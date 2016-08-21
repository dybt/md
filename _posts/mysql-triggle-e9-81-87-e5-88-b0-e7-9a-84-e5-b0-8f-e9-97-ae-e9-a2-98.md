---
title: mysql triggle 遇到的小问题
id: 164
categories:
  - 数据库
date: 2015-07-22 16:33:02
tags:
- 数据库
- mysql 
- mysql triggl
---

<span style="font-size:20px;">&nbsp;&nbsp; &nbsp;今天在做小组的faq的问答用到了mysql,遇到了一点小问题(mysql创建触发器的问题)</span>

	<span style="font-size:20px;">首先创建触发器的格式 : CREATE TRIGGER &lt;trigger_name&gt; [BEFROE][AFTER]+[UPDATE][DELETE][INSERT] ON 表名 FOR EACH ROW BEGIN + sql语句 +END;</span>

	<span style="font-size:20px;">在表myTable中想要实现a字段根据b字段的变化而变化,定义的触发器是</span>

	<span style="font-size:20px;">CREATE TRIGGER &#39;myTrigger&#39; AFTER UPDATE ON myTable FOR EACH ROW BEGIN</span>

	<span style="font-size:20px;">UPDATE myTable SET a=new.b WHERE myTable.b = new.b;这样之后可以编译成功,但是在用的时候会报错,</span>&nbsp;<span style="font-size:20px;">Can&#39;t update table &#39;app_faq_question&#39; in stored function/trigger because it is already used by statement which invoked this stored function/trigger.出现了这个问题,这个问题的原因是循环更新了,网上查了一下资料,说是不要用UPDATE,直接用SET,于是改为</span>

	<span style="font-size: 20px;">CREATE TRIGGER &#39;myTrigger&#39; BEFORE&nbsp;UPDATE ON myTable FOR EACH ROW BEGIN</span>

	<span style="font-size: 20px;">&nbsp;SET a = new.b;最后还是失败了,然后把BEFORE改为AFTER成功.</span>

	&nbsp;

	<span style="font-size: 20px;">还有学到了一点是mysql的timestamp类型,如果你设置:DEFAULT CURRENT_TIMESTAMP的话,它是会默认创建ON UPDATE CURRENT_TIMESTAMP只要你对这一行的记录有所更新,这个值就会更新,所以如果你想要取消掉这个特性,可以设置</span><span style="font-size: 20px; line-height: 20.7999992370605px;">DEFAULT&nbsp;</span><span style="font-size: 20px; line-height: 20.7999992370605px;">CURRENT_TIMESTAMP NULL, 或者直接给初始值 设置为0,还有就是mysql的NOW()函数,返回当前的时间,除此之外,每个表中只能有一个默CURRENT_TIMESTAMP,</span>

	<span style="font-size: 20px;">所以你要实现两个,就要向上面我说的那样,创建一个触发器,来实现同步.</span>

	&nbsp;