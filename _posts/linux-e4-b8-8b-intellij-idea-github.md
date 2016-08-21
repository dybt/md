---
title: Linux下 Intellij idea + github
id: 29
categories:
  - IDE 使用
date: 2015-04-17 13:27:24
tags:
	- Intellij IDEA
	- github
---

## 
	刚在Intellij上实现了github的同步,分享一下

	首先,打开一个工程,或者新建一个工程,然后在工具栏中单击VCS, 然后选中import into version control ,然后选中share project on github,

	(中间输入密码的环节省略),然后会弹出窗口:[![2015-04-17 21:19:36 的截屏](http://leilei.lezifang.cn/wordpress/wp-content/uploads/2015/04/2015-04-17-211936-的截屏-300x156.png)](http://leilei.lezifang.cn/wordpress/wp-content/uploads/2015/04/2015-04-17-211936-的截屏.png)

	New repository 就是你的仓库名,然后下面填描述,然后点集Share,完成本地代码,上传到Github中

	&nbsp;

## 
	远程代码克隆到本地

	点击VCS,然后选中checkout from control version,然后选中github,[![2015-04-17 21:24:10 的截屏](http://leilei.lezifang.cn/wordpress/wp-content/uploads/2015/04/2015-04-17-212410-的截屏-300x119.png)](http://leilei.lezifang.cn/wordpress/wp-content/uploads/2015/04/2015-04-17-212410-的截屏.png)

	Git Repository URL就是你想要拉倒本地的远程仓库的url, Parent Director就是本地目录,然后点击Clone,就可以实现将远程代码克隆到本地.

	&nbsp;

	代码修改.

	git commit( CTRL + K)

	git push ( CTRL + Shift + K )

	也可以点击VCS,然后选中commit 或者push进行git的commit和push命令.