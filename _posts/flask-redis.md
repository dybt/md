title: flask 中使用redis做缓存
date: 2015-12-01 12:10:52
categories: flask
tags: [python, flask]
---
Redis是一款依据BSD开源协议发行的高性能Key-Value存储系统。会把数据读入内存中提高存取效率。Redis性能极高能支持超过100K+每秒的读写频率，还支持通知key过期等等特性，所以及其适合做缓存。

### 下载安装
根据[redis中文网](http://www.redis.cn)使用wget下载压缩包

```bash
$ wget http://download.redis.io/releases/redis-3.0.5.tar.gz
$ tar xzf redis-3.0.5.tar.gz
$ cd redis-3.0.5
$ make
```
二进制文件编译完成后在src目录下。可以通过一下命令启动:
```bash
$ src/redis-server
```
这样就可以看到redis服务已经启动起来了,默认端口是6379,可以通过client操作reids。
```bash
$ src/redis-cli
redis> set foo bar
OK
redis> get foo
"bar"
```

<!--more-->

### flask配置redis
首先得下载flask的缓存插件Flask-Cache，使用pip下载。

```python
sudo pip install flask_cache
```
为应用扩展flask_cache

```python
from flask import Flask
from flask.ext.cache import Cache

cache = Cache()

config = {
    'CACHE_TYPE': 'redis',
    'CACHE_REDIS_HOST': '127.0.0.1',
    'CACHE_REDIS_PORT': 6379,
    'CACHE_REDIS_DB': '',
    'CACHE_REDIS_PASSWORD': ''
}

app = Flask(__name__)
app.config.from_object(config)
cache.init_app(app)

@app.route('/')
@cache.cached(timeout=60*2)
def index():
    name = 'mink'
    return name

if __name__ == '__main__':
    app.run()
```
使用装饰器cached()装饰视图函数，参数timeout来设置过期时间，本文中使用了两分钟为缓存时间。

### 参考
1. [redis中文网](http://www.redis.cn/)
2. [Flask-cache](http://www.pythondoc.com/flask-cache)
