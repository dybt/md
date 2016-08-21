title: Web基础知识系列一：HTTP
date: 2016-07-23 16:26:20
tags:
- Web
- 基础知识
---

### HTTP 协议
+ 简介嘛
  - 上维基百科上看吧

<!-- more -->
### 请求
+ Accept
  - 告诉服务器自己接受什么类型文件，`*/*`表示任何类型，`type/*`表示该类型下面的所有子类型

+ Accept-Encoding
  - 浏览器通知服务器自己所能接收的编码方法，通常是指压缩方法，是否支持压缩，支持何种压缩 `Accept-Encoding:gzip, deflate, sdch`

+ Accept-Language:
  - 浏览器通知服务器自己所接收的语言 `Accept-Language: da, en-gb;q=0.8, en;q=0.7`，这里顺便补充下，里面的q代表了什么，q是一个相关权重系数，范围在0到1之间，越大代表了客户端越希望得到对应的项，如果没有设置的话它的值是1；所以上面的就代表了，da的权重为1,en-db权重为0.8, en权重为0.7；

+ Cookie
  - 告诉服务器客户端的cookie值，用于标识客户端身份，是对HTTP本身无状态的一种弥补方法

+ Connection
  - close: 告诉服务器或者代理服务器，在完成本次请求的响应后，就关闭连接，不要等待本次连接后的后续请求
  - keepalive: 告诉服务器或者代理服务器，在完成本次请求后，保持连接，等待本次连接后的后续请求

+ Host
  - 客户端指定自己想访问的Web服务器的域名或IP地址和端口号

+ User-Agent
  - 客户端表明自己的身份，比如是何种浏览器

+ Referer
  - Referer， 形式如`Referer:http://www.runoob.co`m/nodejs/nodejs-npm.html` 表示当前网页是由`http://www.runoob.com/nodejs/nodejs-npm.html`该网址的网页触发而来的，诸如点击链接之类的
  - 其作用有防盗链和防止恶意请求
  - [参考](http://book.51cto.com/art/201309/411584.htm)
  - [详细参考](http://www.sojson.com/blog/58)
+ Content-Type & Content-Length
  - 说明： 在最初的http协议中，没有 上传文件 方面的功能 ，rfc1867为http协议添加了这个功能，客户端的浏览器按照些规范将用户指定的文件 发送到服务器，服务器端的程序按照些规范，解析聘用记发送来的文件
  - 例子
  ```
  Content-Length:8959
  Content-Type:multipart/form-data; boundary=----WebKitFormBoundaryS7fw1NJRpyKBXTeC
  ```
  
  - Content-Length表示内容的总长度
  - Content-Type里面的`multipart/form-data`表示MIME media类型，boundary的内容表示分隔符，分隔多个文件，表单项

### 响应
+ Cache-Control [参考](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching?hl=zh-cn#cache-control)
  - max-age: 本次响应包含的对象的过期时间
  - public: 可以缓存内容回应任何用户
  - private: 只能用缓存内容回应先前请求该内容的那个用户
  - no-cache: 可以缓存，但是只有在跟服务器验证了其有效性后才能返回给客户端
+ All
  - no-store: 不允许缓存
+ Connection
  - close:连接已经关闭
  - keepalive：连接保持着，在等待本次连接的后续请求
+ Keep-Alive
  - 如果客户端请求保持连接，则该头表示希望服务器保持连接多长时间（秒）
+ Content-type
  - 服务器告诉浏览器自己响应的对象的类型
+ Date
  - 消息发送的时间
+ Expires
  - 表示存在时间，允许客户端在这个时间之前不去检查
+ Vary字段
  - 用于处理HTTP的内容协商机制，有时服务端针对同一个网址地址有多个版本的网页内容，比如为支持压缩的客户端提供压缩版本的，为不支持压缩的提供不压缩的，为不同语言的提供不同的语言版本，此时就需要客户端与服务端进行协商选择最合适的版本了。此时使用的方案是服务端根据客户端发送的请求中某些字段自动发送最合适的版本，专用字段是(Accept字段)
  - 那么对于缓存服务器要如何处理呢，缓存服务器应如何给客户端返回合适的版本呢？这就是Vary的作用了，比如 当其为Accept-Encoding时，当chrome请求了gzip压缩的缓存文件时，那么另一个浏览器浏览此页面，并且这个浏览器不支持压缩，那此此时CDN缓存服务器不会将原来缓存的gzip压缩文件返回，而会向源服务器进行请求新的
  - 另外有时Accept字段并不够用，例如要针对特定浏览器如IE6输出不一样的内容，就需要用到请求头中的User-Agent字段，类似，请求头中的 Cookie 也可能被服务端用做输出差异化内容的依据。所以 HTTP 协议规定，如果服务端提供的内容取决于 User-Agent 这样「常规 Accept 协商字段之外」的请求头字段，那么响应头中必须包含 Vary 字段，且 Vary 的内容必须包含 User-Agent
  - 总之呢，Vary字段就是列出一个响应字段列表，告诉缓存服务器遇到同个URL对应差不同版本文档的情况时，如何缓存和筛选合适的版本
  - 以上参考自 [参考](http://liqinghuan.com/tag/vary)
+ X-cache
  - 表明相应的请求是由代理服务器回的，MISS表示代理未命中，HIT表示代理命中并直接返回

### 关于缓存
+ 首先一个请求到达缓存服务器，缓存服务器首先检查是否已经缓存，检查是否已经过期(相关字段是Expires的绝对时间检查或是Cache-Control:max-age的相对时间,相对Date字段），如果没有过期并符合返回条件，则返回；如果过期的话，缓存服务器需要进行再验证，过期并不一定代表了文档不可用，如果检查之后表示内容没有发生变化，那么只修改响应首部相应信息，如果过期就更换
+ 用条件方法进行再验证
  - HTTP允许缓存向原始服务器发送一个“条件GET”，请求服务器只有在文档与缓存中现有的副本不同时，才回送对象主体，从而实现了将新鲜度检查与对象获取结合成了单个条件GET。使用的头部有`If-modified-Since:<date>`，它与last-modified服务器响应配合使用，来进行是检验；或是使用还有`If-None-Match:<tags>`使用标签(版本)进行检验