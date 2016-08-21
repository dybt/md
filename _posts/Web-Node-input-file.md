title: node 上传文件实现
date: 2016-08-08 21:30:49
tags:
- Web
- Node
---

### 整体介绍
+ 在node里面有多种方式可以实现文件的上传，而且因为有许多模块，所以中间的内容存储等逻辑也都被封装了，往往我们需要的只是进行相应的参数配置和传递回调函数就可以了
+ 前端的话，可以使用form表单的刷新式提交，也可以使用ajax配合FormData进行无刷新式提交
+ 后台的话，可以使用formidable模块，如果使用的是express的话还可以使用multer中间件。

### 代码
+ [github](https://github.com/sysuKinthon/MyWeb/tree/master/inputfile)

### 网址
+ http://itbilu.com/nodejs/npm/NkGKcF14.html
+ https://cnodejs.org/topic/554b824ab68166372e600167

