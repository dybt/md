title: 浏览器专题之缓存url请求
date: 2016-02-27 15:35:17
tags: 
- Browser
- Web
---

#### Get 与 Post的区别
+ Get 请求
    - Get 请求可被爱缓存
    - Get 请求保留在浏览器历史记录中
    - Get 请求可被收藏为书签
    - Get 请求不应用于处理敏感数据
    - Get 请求有长度限制
    - Get 请求只应用于取回数据(幂等)
+ Post 请求
    - Post 请Post求不会被缓存
    - Post 请求不能被收藏为书签
    - Post 请求对数据长度没有要求
+ 根据HTTP规范，GET是用于信息获取的，而且是幂等的,也就是说使用相同的URL重复GET请求会返回预期的相同结果时，GET方法才是适用的；POST更多的作用是修改服务器的信息状态，也就是说POST请求是有副作用的，对于GET请求的幂等性，有的浏览器会默认将url相同的GET请求做缓存处理，比如IE，
![效果](http://7xncgn.com1.z0.glb.clouddn.com/16-2-27/79680014.jpg)
<!-- more -->

#### 问题
+ 不同的浏览器对于相同的get请求所采取的措施也不大一样，有的浏览器会阻塞相同url的请求(相同时间发出的请求),直到当前的get请求完成，而post则不会有些情况,
    - Chrome的情况：发送中阻塞
    ![效果图](http://7xncgn.com1.z0.glb.clouddn.com/16-2-27/20678892.jpg)
    - 请求并获取了数据(关键看数据的请求起始终止时间)
    ![效果图](http://7xncgn.com1.z0.glb.clouddn.com/16-2-27/85749960.jpg)
+ 但有时我们并不希望这种情况的发生，比如我们希望通过一个GET方式去向后台申请一些随机数据时，我们希望每次获得的数据都是不一样的，而且同时发送的,相关测试代码 [Github](https://github.com/sysuKinthon/Web2.0/tree/master/Web2.0/menuCalculate/S3), 如下几种方式解决方式：
    - GET时，在url后面加上查询随机数`$.get('random/?date=' + new Date().getTime(), function() {...})`，让浏览器认为这是不同的请求和资源，而不被缓存或阻塞
    ![效果图](http://7xncgn.com1.z0.glb.clouddn.com/16-2-27/53521270.jpg)
    - 对于不想被缓存的情况，可以在服务器设置响应报文的报文头
```
｛'Content-Type': 'text/plain', "Cache-Control":"no-cache, no-store, must-revalidate"｝
```
    - 使用post请求

+ 参考链接
    - http://java-xp.iteye.com/blog/1518510