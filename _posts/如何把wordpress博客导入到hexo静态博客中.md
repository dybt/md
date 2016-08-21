---
title: 如何把wordpress博客导入到hexo静态博客中
date: 2016-06-24 15:45:09
tags:
 - 博客
 - 博客迁移
 - wordpress
 - hexo
categories:
  - 折腾
---


搭建完**[hexo 博客](http://blog.dreamleilei.com/2016/06/21/createGithubPage/)**之后，准备把之前自己用*wordpress*搭建的博客迁移到
**hexo**博客中，之前想过其它办法，最无脑的办法，就是把之前的文章，copy
然后paste，但是这种做法不适合于大量的文章，我们可以通过hexo的wordpress插件
,把之前自己的wordpress博客导出为xml文件，然后通过命令生成md文件，这样就实现
了把wordpress博客迁徙到hexo静态博客中。

## 步骤
1. 添加**hexo-migrator-wordpress**插件
 ```bash
npm install hexo-migrator-wordpress --save
 ```
2. 把自己的wordpress博客中的文章导出为xml文件
 在自己的wordpress博客里，找到工具，然后导出格式为xml文件。
 wordpress导出xml文件参考下图:
![图片](http://blogpicture.dreamleilei.com/blog-my-pic.png)

3. 通过工具把xml文件转换为多个md文件
```bash
hexo migrate wordpress wordpress.xml
```

## 参考链接
> http://www.chinaz.com/web/2014/0610/354945.shtml


    
