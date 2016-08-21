---
title: Linux下java的JNI技术(入门)
id: 159
categories:
  - Java JNI入门
date: 2015-07-15 11:00:36
tags:
	- Java
	- JNI
---

<span style="font-size:20px;">&nbsp; &nbsp; &nbsp; 今天下午,舍友都已经离校了,宿舍里只剩下了两个人,于是我想到了如果另一个舍友也离开之后我一个人,我的联通网怎么激活上网(我用的是Linux系统),于是我想起了之前我们学校贴吧曾经公开的激活工具的源码,核心代码是用C写的,于是我想可不可以自己写了Java程序,这样就可以跨平台激活了,然而要用C作为核心,于是我就想到了JNI技术.</span>

	<span style="font-size:20px;">&nbsp;</span>&nbsp; &nbsp; &nbsp;&nbsp;<span style="font-size:20px;">好了,闲话不多说,开始进入我们的正题,JNI技术<span style="font-family: arial, 宋体, sans-serif; line-height: 24px; text-indent: 28px;">是Java Native Interface的缩写，它提供了若干的</span>[API](http://baike.baidu.com/subview/16068/5889234.htm)<span style="font-family: arial, 宋体, sans-serif; line-height: 24px; text-indent: 28px;">实现了Java和其他语言的通信（主要是</span>[C](http://baike.baidu.com/subview/10075/6770152.htm)<span style="font-family: arial, 宋体, sans-serif; line-height: 24px; text-indent: 28px;">&amp;</span>[C++](http://baike.baidu.com/view/824.htm)<span style="font-family: arial, 宋体, sans-serif; line-height: 24px; text-indent: 28px;">）,Object类中的clone方法就是native方法.</span></span>

	&nbsp;

	<span style="font-size:20px;"><span style="font-family: arial, 宋体, sans-serif; line-height: 24px; text-indent: 28px;">&nbsp;&nbsp;首先,创建一个Java文件,然后如果想要用到本地的方法,就是简单的声明方法,而不用实现,记得要加上native.</span></span>

	[![2015-07-15 17:58:04 的截屏](http://www.dreamleilei.com/wordpress/wp-content/uploads/2015/07/2015-07-15-175804-的截屏-300x135.png)](http://www.dreamleilei.com/wordpress/wp-content/uploads/2015/07/2015-07-15-175804-的截屏.png)

	<span style="font-size: 20px; line-height: 32px;">例如,我写的这个sayHelloWorld方法就是一个本地的方法,完成之后,在命令行下调用javah命令. javah -jni TestJni (这里需要注意的是后面跟的参数是类名而不是文件名)</span>

	<span style="font-size: 20px; line-height: 32px;">然后你会发现生成了一个和你的类名相同的.h文件,打开文件,格式如下:</span>

	[![2015-07-15 18:01:11 的截屏](http://www.dreamleilei.com/wordpress/wp-content/uploads/2015/07/2015-07-15-180111-的截屏-300x159.png)](http://www.dreamleilei.com/wordpress/wp-content/uploads/2015/07/2015-07-15-180111-的截屏.png)

	<span style="font-size:20px;">请看第15和第16行,这就是你要写的函数的声明,刚才的命令已经把格式写好,你只需要创建相应的C文件或CPP文件即可(c文件和cpp文件命名无要求,但是必须包含之前生成的.h文件),我在这里创建的是sayHello.cpp</span>

	<span style="font-size:20px;">内容如下:</span>

	[![2015-07-15 18:04:52 的截屏](http://www.dreamleilei.com/wordpress/wp-content/uploads/2015/07/2015-07-15-180452-的截屏-300x102.png)](http://www.dreamleilei.com/wordpress/wp-content/uploads/2015/07/2015-07-15-180452-的截屏.png)

	&nbsp;

	<span style="font-size:20px;">你会发现该函数没有main函数,然后函数的返回值和参数以及函数名与之前生成的.h文件的是一样的,这个是必须的,然后里面写具体的实现就好.</span>

	&nbsp;

	<span style="font-size:20px;">&nbsp;&nbsp; &nbsp; 写完之后,需要编译生成动态链接库,(Linux下的动态链接库的格式为libXXXX.so),用gcc 命令 gcc -shared -fPIC -I /path&nbsp;-I /path/ -I /usr/include -I ./ xxx.c -o libxxx.so (其中path是路径,一般在jdk下的include 里面包含头文件jni.h 在该目录下 有一个linux子目录,里面有jni_md.h这两个文件是我们所需要的,/usr/include 是我们C和CPP文件中一些必要的头文件,然后./表示是当前路径下生成.h文件 -o 表示生成的动态链接库的名称, -fPIC&nbsp;<span style="color: rgb(0, 0, 0); font-family: Helvetica, Tahoma, Arial, sans-serif; line-height: 25.2000007629395px;">：表示编译为位置独立的代码，不用此选项的话编译后的代码是位置相关的所以动态载入时是通过代码拷贝的方式来满足不同进程的需要，而不能达到真 正代码段共享的目的.&nbsp;</span></span>

	<span style="font-size:20px;"><span style="color: rgb(0, 0, 0); font-family: Helvetica, Tahoma, Arial, sans-serif; line-height: 25.2000007629395px;">&nbsp;&nbsp; &nbsp; 接下来只需要导入这个库即可,导入的时候用System.loadLibrary(&quot;库名&quot;) (比如你之前生成的库名叫做libxxx.so,这里库名要填xxx不要前面的lib和后面 的.so).</span></span>

	[![2015-07-15 17:58:04 的截屏](http://www.dreamleilei.com/wordpress/wp-content/uploads/2015/07/2015-07-15-175804-的截屏-300x135.png)](http://www.dreamleilei.com/wordpress/wp-content/uploads/2015/07/2015-07-15-175804-的截屏.png)

	&nbsp;

	<span style="font-size:20px;">最后java 类名,运行,可能报异常这个时候要用java -Djava.library.path=./ 类名或者把相应的动态链接库复制到相应的u路径下即可.</span>