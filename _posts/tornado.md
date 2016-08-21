title: tornado 简单事例
date: 2015-02-28 12:45:44
categories: tornado
tags: [tornado, python]
---
tornado 是一个区别于其他python框架的非阻塞服务器。
### 下载和安装
ubuntu下首先得下载pip ,pip是python的一种包管理工具
``` bash
$ sudo apt-get install python-pip
```
下载完以后就可以通过 pip来进行安装.(大部分python模块都可以通过其安装)
``` bash
$ sudo pip install tornado
```
<!--more-->
可通过python的交互模式，查看模块的安装情况
### Hello, World
以下是hello, world事例
```python    
import tornado.ioloop
import tornado.web

class MainHandler(tornado.web.RequestHandler):
    def get(self):
        self.write("Hello, world")

application = tornado.web.Application([
    (r'/', MainHandler),
])

if __name__ == '__main__':
    application.listen(8888)
    tornado.ioloop.IOLoop.instance().start()
```
现在试试在浏览器上输入<http://localhost:8888>来访问你的web服务吧.
