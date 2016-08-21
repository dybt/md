---
title: 从 Octopress 迁移到 Hexo 后的一些遗留问题
date: 2016-03-03 18:00:12
tags:
- Hexo
- Octopress
categories: Other
---

上周我讲自己的博客从 Octopress 迁移到了 Hexo，感觉还是很不错的，但是由于Octopress 和 Hexo 存在一些差别，所以导致了如下三个问题：

1. 以前 Octopress 的网址是包含 /blog/ 的，迁移之后没了，这样会导致由搜索引擎（搜索引擎更新还是很不及时的）的链接和外网引用的链接点过来会跳到404页面
2. 生成的网址区分大小写了，以前全部是小写的，这会导致以前的小写链接无法打开相应的页面（如 `categories/xcode`，现在是 `categories/Xcode`）
3. 之前的一些文件遗失了（其实也就一个）

今天就来说一下我是怎么解决这三个问题：

<!-- more -->

### 第一个问题

需要自定义一个 404 页面（在 `source` 目录下创建一个 404 页面_[参考](http://ibruce.info/2013/11/22/hexo-your-blog/#404页面)_）。本来想做一下404公益的，但是由于[腾讯公益404](http://www.qq.com/404)是完全替换了我的404页面的内容，无法达到我的效果，所以舍弃了；[404公益_益云(公益互联网)社会创新中心](http://yibo.iyiyun.com/Index/web404)倒是可以，但是在没有适应移动设备（宽高限定死了），所以也舍弃了；[失蹤兒童少年資料管理中心404](http://404page.missingkids.org.tw/)是台湾的网站，没有试，所以也舍弃了。

创建好页面之后在文件中写入如下内容：

```html
---
title: 页面没有找到
description: 页面没有找到
comments: false # 关闭评论功能
---

<h1>页面没有找到</h1>

<ul id="back-link">
  <li><a href="/">返回主页</a></li>
</ul>
<script>
  var originalLocation = window.location;
  if (originalLocation.pathname.indexOf('/blog/') == 0) {
    var originalHref = originalLocation.href;
    var pathIndex = originalHref.indexOf(location.pathname);
    // 因为 /blog/ 长度是 6，而且包含了两个 /，所以要减掉一个常晒
    var newHref = originalHref.substr(0, pathIndex) + originalHref.substr(pathIndex + 5)
    var liTag = document.createElement('li');
    var aTag = document.createElement('a');
    aTag.setAttribute('href', newHref);
    aTag.text = '也可能移到了这里：' + newHref;
    liTag.appendChild(aTag);
    var blankLink = document.getElementById('back-link');
    blankLink.appendChild(liTag);
  }
</script>
```

之后再重新生成和发布就可以了。

上述代码的作用就是现实一个无序列表，里面有一条“返回主页”链接，通过 js 判断当前网址的路径是否以 `/blog/` 开头，如果是则创建一个链接，把 `/blog/` 去掉并生成新的网址插入到无序列表中。

### 第二个问题

首先需要改一下配置文件

```yaml
# 将此选项设置成 1， 设置成 1 的意思是全部转成小写
filename_case: 1
```

如果之前有部署再传上去是会有问题的，虽然之前的大写字母现在已经全部换成了小写，但是传不到 [Github](https://github.com) 上，可能是 git 忽略了大小写吧。我的方法是直接把原来的仓库删掉重建一遍，然后再上传，反正也无所谓（本地记得删除 `.deploy_git` 目录和执行 `hexo clean` 命令）。

### 第三个问题

这个问题就更简单了，直接找到原来的网站备份，把遗失的文件和目录上传到相应的地方（`source` 目录下）就行了。

