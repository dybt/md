title: 实现图片的延迟加载和预加载
date: 2016-03-26 09:43:42
tags: Web
---
### 实现懒加载

#### 相关原理与实现
+ 关于实现的原理和使用的必要性可以参考 [原理](http://www.tuicool.com/articles/rUjIZzb)
+ 关于代码实现可以参考 [代码](http://www.cnblogs.com/tugenhua0707/p/3515292.html), 另外根据[博主](http://www.cnblogs.com/tugenhua0707/p/3515292.html)的代码，我也稍微进行了点自己的修改和加上部分注释，但代码基本还是[博主](http://www.cnblogs.com/tugenhua0707/p/3515292.html)的代码,此处仅供参考 [Github源码](https://github.com/sysuKinthon/imgLazyLoad)
+ jQuery实现的[源码](https://raw.githubusercontent.com/tuupola/jquery_lazyload/master/jquery.lazyload.js)

### 实现预加载
+ 原理参考并对比于懒加载 [原理](http://www.cnblogs.com/tugenhua0707/p/3525668.html)，主要是认识到**设置其src来实现预载，再使用onload方法回调预载完成事件。只要浏览器把图片下载到本地，同样的src就会使用缓存，这是最基本也是最实用的预载方法。**
+ 参考代码 [Github]()

### 参考链接

#### 懒加载
+ http://www.tuicool.com/articles/rUjIZzb 
+ http://stylechen.com/imglazyload2.html
+ http://www.cnblogs.com/tugenhua0707/p/3515292.html
+ http://www.w3cways.com/1765.html

#### 预加载
+ http://blog.csdn.net/leftfist/article/details/46805387
+ http://www.cnblogs.com/mz121star/archive/2012/11/01/javascript_preloadimages.html




