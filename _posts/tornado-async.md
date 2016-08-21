title: tornado 简单异步非阻塞
date: 2015-05-15 11:45:28
categories: tornado
tags: [tornado, python, 异步非阻塞]
---
**tornado**即是一个http非阻塞服务器, 就要用起来, 我们将用到`tornado框架` ,`mongodb数据库` 以及`motor(mongodb的异步驱动)`.来简单实现tornado的非阻塞功能.

### 下载与安装
安装mongodb
``` bash
$ sudo apt-get install update
$ sudo apt-get install mongodb
```
安装motor
``` bash
$ pip install motor
```

### 非阻塞
```python
# conf.py

import os
import motor
from handlers import index, auth

BASE_DIR = os.path.join(__file__)

handlers = [
    (r'^/$', index.IndexHandler),
    (r'^/auth/register$', auth.RegisterHandler),
    (r'^/auth/login$', auth.LoginHandler),
]

settings = dict(
    debug = True,
    template_path = os.path.join(BASE_DIR, 'templates'),
    static_path = os.path.join(BASE_DIR, 'static'),
)

client = motor.MotorClient("127.0.0.1")
db = client.meet
```
首先在配置文件中连接数据库, `client.db_name`中 `db_name`就是数据库的名称.更多信息点击[motor文档][1]
[1]: http://motor.readthedocs.org/en/stable/index.html "motor文档"
<!--more-->
```python
# handlers/__init__.py
class BaseHandler(tornado.web.RequestHandler, TemplateRendering):
    def initialite(self):
        ...

    @property
    def db(self):
        return self.application.db
```
添加`db()`并使用`property`装饰,像属性一样访问数据库.
```python
# auth.py

import os 
import time 
import tornado.web
from tornado import gen
from . import BaseHandler

class RegisterHandler(BaseHandler):
    def get(self):
        self.render_html('register.html')

    @tornado.web.asynchronous
    @gen.coroutine
    def post(self):
        username = self.get_argument('username', None)
        email = self.get_argument('email', None)
        password = self.get_argument('password', None)

        data = {
            'username': username,
            'email': email,
            'password': password,
            'timestamp': time.time() * 1000,
        }

        if username and email:
            yield self.db.user.insert(data)
        self.redirect('/')

class LoginHandler(BaseHandler):
    
    @tornado.web.asynchronous
    @gen.coroutine
    def get(self):
        username = self.get_argument('useranme')
        user = yield self.db.user.find_one({'username': username})
        self.render_html('login.html', user=user)
```
`@gen.coroutine`装饰使函数非阻塞, 返回一个生成器, 而不用在使用回调函数. motor也通过yield 实现异步(不然还得返回一个回调函数). 其实这个例子反映不了阻塞问题关键是时间太短.
我们修改一下代码
```python
# 之前
yield self.db.user.insert(data)

# 之后
yield tornado.gen.Task(tornado.ioloop.IOLoop.instance().add_timeout, time.time() + 10)
```
这里通过`tornado.ioloop.IOLoop.instance().add_timeout`阻塞应用, 这是`time.sleep`的非阻塞实现, 如果这里使用`time.sleep`因为是tornado是单线程会阻塞整个应用所以别的`handler`也无法访问. 
可以看到我在注册页面注册后,在阻塞期间点击`/auth/login`直接就访问了`login`页完成非阻塞.
