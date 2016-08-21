title: tornado 异步下redirect问题
date: 2015-05-19 13:53:59
categories: tornado
tags: [tornado, python, 异步非阻塞]
---
在使用`tornado`的时候常常遇到一些问题, 特将遇到的问题和解决的方法写出来(这里的感谢一下帮我解答疑惑的pythonista们)

### 问题
我想要实现一个注册用户功能, web框架使用`tornado`数据库使用`mongodb`但在注册时出现`Exception redirect`的错误. 现贴下代码:
```python
class Register(BaseHandler):
    def get(self):
        self.render_html('register.html')

    @tornado.web.aynchronous
    @gen.coroutine
    def post(self):
        username = self.get_argument('username')
        email = self.get_argument('email')
        password = self.get_argument('password')
        captcha = self.get_argument('captcha')

        _verify_username = yield self.db.user.find_one({'username': username})
        if _verify_username:
            self.flash(u'用户名已存在', 'error')
            self.redirect('/auth/register')

        _verify_email = yield self.db.user.find_one({'email': email})
        if _verify_email:
            self.flash(u'邮箱已注册', 'error')
            self.redirect('/auth/register')

        if captcha and captcha == self.get_secure_cookie('captcha').replace(' ',''):
            self.flash(u'验证码输入正确', 'info')
        else:
            self.flash(u'验证码输入错误', 'error')
            self.redirect('/auth/register')

        password = haslib.md5(password + self.settings['site']).hexdigest()

        profile = {'headimg': '', 'site': '', 'job': '', 'signature':'',
                    'github': '', 'description': ''}
        user_profile = yield self.db.profile.insert(profile)
        user = {'username': username, 'email': email, 'password': password,
                'timestamp': time.time(), 'profile_id': str(user_profile)}

        yield self.db.user.insert(user)
        self.set_secure_cookie('user', username)
        self.redirect('/')
```

<!--more-->
>本想如果用户验证码输入出错就跳转到注册页面, 但问题是验证码出错也会继续执行一下代码. 虽然在`self.redirect`后加上`self.finish`会终止代码,但是因为`self.redirect` 函数内已有`self.finish`所以出现了两次报出异常终止的代码.
因为以上原因代码不会被终结, 验证码出错用户还是会注册.

### 解决方案
```python    
return self.redirect('/auth/register')
或
self.redirect('/auth/register')
return
```
----------
[segmentdefault][1]中热心用户**rsj217**给出的答案
`self.finish` 会关掉请求, 因为`@tornado.web.aynchronous`告诉`tornado`会一直等待请求(长链接). `self.redirect`等于设置了`response`的`headers`的`location`属性. 
    
[segmentdefault][1]中热心用户**依云**给出的答案
`self.finish`当然不会跳出函数, 不然请求结束之后还想做些事情怎么办呢.
[1]: http://segmentfault.com/q/1010000002777238 "segmentdefault"

### 总结
因为错把`self.finish`当做跳出函数出现了以上的问题

* `self.redirect`会在`request.headers` 里设置`location`用于跳转
* `self.finish`会关掉请求, 但不会跳出函数
