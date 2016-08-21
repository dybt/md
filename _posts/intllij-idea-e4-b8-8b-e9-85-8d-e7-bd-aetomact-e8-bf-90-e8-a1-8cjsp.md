---
title: Intllij idea 下配置tomact运行jsp
id: 64
categories:
  - IDE
date: 2015-05-30 01:11:40
tags: 
	- Intellij IDEA
	- tomcat 
---

<span style="font-size:14px;">昨天接到一个突如其来的好消息,就是我的甲骨文杯的比赛进了决赛,本来是要被刷掉的,可是因为前面的队伍存在这样那样的问题,所以取消了他们的比赛资格,就这样,我们的三人小分队就顺利的进入了决赛(虽然我很水,靠的是宇神和伟神),不过这样让我头疼了,本来想搞前端开发的我就这样要学java了(自从知道自己没进决赛,已经两星期没看java了),既然要学java,那就有必要进阶一下 ,学完java基础,学点java web的东西,从事java web应用开发,既然要学java web应用开发,自然用到JSP和Servlet,既然用到这两个,自然就离不开tomact,既然用到tomact,就必然要用到idea配置tomact环境.</span>

	<span style="font-size:14px;">好了,废话不多说,现在开始我的苦逼旅程之路.</span>

	<span style="font-size:14px;">首先打开File &gt; Setting &gt; Build Execution Delopyment 然后点击右上角的+ 选择tomact server 然后配置好自己的tomact路径.</span>

	<span style="font-size:14px;">[![2015-05-30 08:59:53 的截屏](http://www.dreamleilei.com/wordpress/wp-content/uploads/2015/05/2015-05-30-085953-的截屏-269x300.png)](http://www.dreamleilei.com/wordpress/wp-content/uploads/2015/05/2015-05-30-085953-的截屏.png)</span>

	<span style="font-size:14px;">第二步:</span>

	<span style="font-size:14px;">打开File &gt; Project Structure &gt; Modules ,然后点击右边的Dependence,点击+ 选择2Library,添加进tomact的安装包即可</span>

	<span style="font-size:14px;">[![2015-05-30 09:04:06 的截屏](http://www.dreamleilei.com/wordpress/wp-content/uploads/2015/05/2015-05-30-090406-的截屏-300x172.png)](http://www.dreamleilei.com/wordpress/wp-content/uploads/2015/05/2015-05-30-090406-的截屏.png)</span>

	<span style="font-size:14px;">第三步:</span>

	<span style="font-size:14px;">[​](http://www.dreamleilei.com/wordpress/wp-content/uploads/2015/05/2015-05-30-090723-的截屏.png)<span style="line-height: 20.7999992370605px;">打开Run &gt; Edit Congitures进行配置即可.</span>[![2015-05-30 09:07:16 的截屏](http://www.dreamleilei.com/wordpress/wp-content/uploads/2015/05/2015-05-30-090716-的截屏-300x193.png)](http://www.dreamleilei.com/wordpress/wp-content/uploads/2015/05/2015-05-30-090716-的截屏.png)</span>

	<span style="font-size:14px;">[![2015-05-30 09:07:23 的截屏](http://www.dreamleilei.com/wordpress/wp-content/uploads/2015/05/2015-05-30-090723-的截屏-300x189.png)](http://www.dreamleilei.com/wordpress/wp-content/uploads/2015/05/2015-05-30-090723-的截屏.png)</span>

	&nbsp;

	<span style="font-size:14px;">第四部:</span>

	<span style="font-size:14px;">在左边的文件中找到要运行的jsp文件,右击Run即可.</span>

	&nbsp;

	<span style="font-size:14px;">注意事项:</span>

	<span style="font-size:14px;">配置tomact的时候可能因为权限的问题,idea找不到tomact记得权限一定要给够</span>

	&nbsp;