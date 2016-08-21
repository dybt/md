title: python实现网上书店
date: 2016-02-28 09:08:48
tags: Project
---
 
#### **需求**
+ 每个用户的 购物车（cart） 用合适的数据结构缓存在内存中。
+ 对每个数据对象的访问，合理设置并管理缓存（使用独立的模块读写数据库）。让程序结构变得可管理、可维护。
+ 对每种商品，设定一个库存。下订单就（假设不需要付款）完成需要减库存。
+ 将商品详情页面的当前库存采用 ajax延迟加载。

#### **实现**
+ 数据库使用mongodb,同时使用redis将部分数据缓存在内存中
+ 相关的数据设计
    - ![user](http://7xncgn.com1.z0.glb.clouddn.com/16-2-28/68616210.jpg)
    - ![book](http://7xncgn.com1.z0.glb.clouddn.com/16-2-28/2234544.jpg)
    - ![cart](http://7xncgn.com1.z0.glb.clouddn.com/16-2-28/44972767.jpg)
+ 代码链接 [Github](https://github.com/sysuKinthon/bookstore/tree/master/bookstore)
#### **参考链接**
+ tornado(mongodb与模版): http://demo.pythoner.com/itt2zh/
+ redis操作：https://redis-py.readthedocs.org/en/latest/index.html