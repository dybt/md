---
title: WordPress搭建个人博客
id: 37
categories:
  - 博客搭建
date: 2015-04-19 15:05:39
tags:
    - 博客
---

# 
	<span style="font-size:18px;"><span style="font-family:arial,helvetica,sans-serif;">刚刚用wordpress成功的搭建了自己的个人博客主页,分享一下</span></span>

	<span style="font-size:18px;"><span style="font-family:arial,helvetica,sans-serif;">条件:虚拟主机+域名</span></span>

	<span style="font-size:18px;"><span style="font-family:arial,helvetica,sans-serif;">首先你要有一个独立的空间,比如你可以申请万网的虚拟主机或者申请一个虚拟的云服务器之类的,总之你需要有一个空间放wodrpess,本人使用的万网提供的虚拟主机,小伙伴们如果也想要申请一下的话可以登录到_**http://www.net.cn/hosting/free/**_&nbsp;来申请自己的个人虚拟空间(不过每个人申请一个免费的,而且期限只有一年),申请的过程也不是太繁琐,就是需要各种拍照,各种认证.</span></span>

	<span style="font-size:18px;"><span style="font-family:arial,helvetica,sans-serif;">申请到虚拟主机之后登录到万网查看自己的主机信息,大约如下</span></span>

	<span style="font-size:18px;"><span style="font-family:arial,helvetica,sans-serif;">[![test](http://leilei.lezifang.cn/wordpress/wp-content/uploads/2015/04/test-300x109.png)](http://leilei.lezifang.cn/wordpress/wp-content/uploads/2015/04/test.png)</span></span>

	<span style="font-size:18px;"><span style="font-family:arial,helvetica,sans-serif;">以后你就可以通过ftp登陆到相应的站点管理自己的空间了,有了这个之后,你去网上下载一个wordpress的压缩包,用ftp 上传到你的htdocs目录下,然后在万网登录自己的空间,解压一下,然后安装wordpress(网上教程和博文好多,此处不再重复),记得我当时遇到的问题是数据库的登录地址,不要填localhost要填万网提供的那个域名.</span></span>

	<span style="font-size:18px;"><span style="font-family:arial,helvetica,sans-serif;">安装完成后,你需要将自己的域名解析到自己的虚拟主机的ip,你可以在dns.la网站上设置免费的域名解析服务,我在这里不再啰嗦.</span></span>

## 
	<span style="font-size:18px;"><span style="font-family:arial,helvetica,sans-serif;">分享一下安装完wordpress后做的几件事:</span></span>

	<span style="font-size:18px;"><span style="font-family:arial,helvetica,sans-serif;">首先安装主题,主题有好多,你可以根据自己的喜好选择你所喜欢的主题.</span></span>

	<span style="font-size:18px;"><span style="font-family:arial,helvetica,sans-serif;">然后安装一些必要的插件.</span></span>

	<span style="font-size:18px;"><span style="font-family:arial,helvetica,sans-serif;">然后完善一下自己的资料啥的.</span></span>

	<span style="font-size:18px;"><span style="font-family:arial,helvetica,sans-serif;">我安装的插件:</span></span>

	<span style="font-size:18px;"><span style="font-family:arial,helvetica,sans-serif;">FCEditor 一款不错的文本编辑插件,让你的编辑更加得心应手.</span></span>

	<span style="font-size:18px;"><span style="font-family:arial,helvetica,sans-serif;">wp-player 为了让自己得博客更加炫酷,可以实现博客中添加音乐的功能,具体的安装操作网上也有比较详细的教程.</span></span>

	<span style="font-size:18px;"><span style="font-family:arial,helvetica,sans-serif;">simple local avatars 这个插件可以让你上传自己的头像.</span></span>

	<span style="font-size:18px;"><span style="font-family:arial,helvetica,sans-serif;">为了让自己得首页更加的多彩,我还条件了新浪微博的挂件,和虾米音乐.</span></span>

	<span style="font-size:18px;"><span style="font-family:arial,helvetica,sans-serif;">新浪微博的挂件安装方法:http://app.weibo.com/tool/weiboshow在新浪微博的小工具里找到微博秀,然后他会给你提供HTML的代码,在wordpress的自定义,小工具里添加文本然后粘贴相应的HTML代码,即可实现新浪微博的挂件功能.</span></span>

	<span style="font-size:18px;"><span style="font-family:arial,helvetica,sans-serif;">虾米音乐的实现方法:登录虾米音乐的官方网站,然后搜索到o歌曲后点击more选择ai转帖到其它网站g同样也会得到一段HTML代码,根据以上类似的方法就可以实现首页的音乐播放器.</span></span>
