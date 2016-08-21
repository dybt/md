title: flask 简单事例
date: 2015-12-01 11:00:14
categories: flask
tags: [python, flask]
---
Flask是一个Python实现的Web开发微框架。

### 下载和安装
```bash
$ sudo apt-get install python-pip
$ sudo apt-get install flask
```
就可以安装最新的flask了。

### 最小的应用
一个最小的Flask应用看起来会这样。

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello Wrold!'

if __name__ == '__main__':
    app.run()
```

将这段代码保存程一个`.py`文件然后命令行运行代码.

```python
$ python xxx.py
  * Running on http://127.0.0.1:5000
```

<!--more-->

1. 首先我们在代码的第一行导入了Flask类，这个类的实例将是WSGI的应用程序。(也就是可调用的callable。)
2. 第三行代码创建了一个类的实例并，并把`__name__`传给类这样Flask就能找到应用的入口了。
3. route() 装饰器制定URL。
4. run()函数让应用运行在本地服务器。

### 外部可访问
app.run(host='0.0.0.0'), 这样一个局域网内都可以访问你的应用了。

### 调试模式
有两种方法

```python
app.debug = True
app.run()
```
另一种是当做run()函数的参数
```python
app.run(debug=True)
```

### 路由
我们通过route()装饰器把一个函数绑定到对应的URL上。

```python
@app.route('/')
def index():
    return 'Index Page.'

@app.route('/hello')
def hello():
    return 'Hello World'
```
也可以构造动态部分URL
```python
@app.route('/user/<username>')
def show_user_profile(username):
    return 'User %s' % username

@app.route('/post/<int:post_id')
def show_post(post_id):
    return 'Post %d' % post_id
```
通过在标记中加上类型规则就可以制定类型了。

类型|说明
---|---
int|接受整数
float|接受浮点数
path|和默认的相似

### 构造URL
Flask可以通过url_for()来构造指定的url。

```python
>>> from flask import Flask, url_for
>>> app = Flask(__name__)
>>> @app.route('/login')
... def login(): pass
>>> with app.test_request_context():
...     print url_for('login')
...
/login
```

### HTTP方法
HTTP有许多不同访问URL的方法。默认路由只回应GET请求, 但通过route()装饰器传递methods参数改变行为。
```python
@app.route('/', methods=['GET', 'POST'])
def index():
    if request.method == 'POST':
        do_something()
    else:
        do_something()
```

### 渲染模板
由Flask渲染模板是通过, Jinja2模板引擎。(Flask和Jinja2都是相同作者), 你需要调用render_template方法

```python
from flask import render_template 

@app.route('/hello')
@app.route('/hello/<name>')
def hello(name=None)
    return render_template('hello.html', name=name)
```

hello.html必须在这个应用的templates目录中。(Flask会寻找名为templates的目录)

### 请求对象
跟大部分WSGI应用一样Flask使用`request`作为请求的关键字。要处理请求需要调用request，如果请求通过POST或PUT提交数据可以通过request.form提取。GET方法的提交数据可以通过request.args来提取。

```python
from flask import request

@app.route('/login', method=['GET', 'POST'])
def login:
    error = None
    if request.method == 'POST':
        if valid_login(request.form['username'],
                       request.form['password']):
            return login(request.form['username'])
        else:
            error = 'Invalid username/password'

    return render_template('login.html', error=error)
```

### 重定向和错误
Flask的redirect()函数把用户重定向到其他地方。放弃请求并返回错误代码用abort()函数。

```python
from flask import abort, redirect, url_for

＠app.route('/')
def index():
    return redirect(url_for('login'))

@app.route('/login')
def login():
    abort(401)
    this_is_never_executed()
```
这样就可以重定向和禁止访问了，当然有时候访问不存在的页面时会报错，这个时候就需要处理请求。errorhander()装饰器可以处理这些请求。

```python
from flask import render_template

@app.errorhandler(404)
def page_not_found(error):
    return render_template('404.html'), 404
```

### 参考
1. [Flask快速入门](http://docs.jinkan.org/docs/flask/quickstart.html#a-minimal-application)
