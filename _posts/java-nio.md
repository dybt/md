---
title: Java nio
id: 112
categories:
  - Java复习
date: 2015-06-04 13:18:26
tags:
    - Java
    - nio
---

## 
	<span style="font-size:20px;">最近再看I/O的一些东西,写出来,感觉有点水,仅供自己复习的时候看</span>

	<span style="font-size:20px;">前面基本的I/O流都差不多有看过,根据字节流和字符流也是比较容易区分的,但是有时候需要写入固定格式的数据,这时候就有点头疼,比如说我要写书10条double类型的数据和10条int类型的数据到文件中,用基本的输入输出流有点麻烦,必须把double和int类型转换成String才能写入,这个时候就可以用到DataInputStream 和 OutputStream的字节流来写入,如果想要用字符流写入Int和double类型,用PrintWriter可以方便的写入,不过读出的时候却比较麻烦.</span>

	&nbsp;

	<span style="font-size:20px;">还有之前遗忘的一点就是Sytstem.setIn(InputStream) System.setOut(InputStream) System.setErr(inputStream)这三个是系统的输入输出流重定向用的.</span>

	&nbsp;

	<span style="font-size:20px;">字节流和字符流的区别除了基本单位不同外,我还学到了一点就是字符流是通过系统的缓冲区为中介的,如果你用字符流向文件写入的时候忘记了close(),你会那发现文件中并没有写入数据,这是因为字符流中读入的数据仍然存在系统的缓冲区中,这是你需要flush或者close掉字符流,你就会发现已经存在里面了.</span>

	&nbsp;

	<span style="font-size:20px;">对象的序列化也是I/O的比较重要的一个部分,不过没有仔细的研究过.</span>

	&nbsp;

	<span style="font-size: 20px; line-height: 32px;">字节流转换为字符流的InputStreamReader 和OutputStreamWriter可以很好的帮助我们从键盘中输入汉字,然后以字符的形式写入到另一个流中(比如说写入到文本中,此时不会出现乱码)</span>

	&nbsp;

	<span style="font-size: 20px; line-height: 32px;">最近还有在看一些NIO的东西</span>

	<span style="font-size: 20px; line-height: 32px;">nio中的BtyeBuffer可以人为的去创建一段缓冲区,通过调用ByteBuffer.allocate(size)来创建,除了ByteBuffer外,其他类型的Buffer也都有,不过ByteBuffer比较常见.</span>

	&nbsp;

	<span style="font-size: 20px; line-height: 32px;">nio中多了的东西叫做Channel的,FileInputStream, FileOutputStream和RandomAcessFile,都可以调用getChannel()方法,产生一个通道,利用这个特性可以提高IO读写的速度</span>

	&nbsp;

	<span style="font-size: 20px; line-height: 32px;">read() 和write()方法中可以直接以自己定义的缓冲区作为目标,当缓冲区从读状态变成写入的时候,一定要先调用flip()这个方法把limit设置为position,然后把position设置为0,然后写入的时候刚好把要写入的内容写入,然后调用clear方法,这是把position设置为0,limit设置为capcity,又实现了从0开始读数据.</span>
