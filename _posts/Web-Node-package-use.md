title: Node包使用
date: 2016-07-23 16:12:57
tags:
- Web
- Node
---
> 这个博文是用于记录某些使用过的node包

### mconfig包
+ 介绍
  - 用于加载配置文件的，如果需要在项目中使用大量配置变量的话，或为不同的包引入不同的配置文件的话，可以使用这个来统一管理配置文件
+ 使用注意 
  - 默认情况下，如果只是config = require('mconfig')()的话，那么它会直接加载 ./config/my.defaults.conf.js的话，如果是config1 = require('mconfig')('file1')的话，而且file1文件存在，那么会先加载./config/my.default.conf，再加载file1的，如果file1不存在的话，那么只会加载./config/my.default.conf.js

### mload
+ 介绍
  - 用于加载别名的

### domain
+ 介绍
  - 因为在try-catch里面无法做到捕捉异步出现的异常并进行统一处理，所以引入domain可以 捕捉到
  - 具体[参考](https://cnodejs.org/topic/516b64596d38277306407936)