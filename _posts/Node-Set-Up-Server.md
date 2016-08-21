title: 利用nodejs搭建服务器实现注册登录系统
date: 2016-02-27 16:04:54
tags:
- Web
- Project
---

### **要求**
+ 可用node signin.js启动程序，然后通过浏览器 http://localhost:8000/ 访问
+ 有两个界面“注册”、“详情”，使用者通过在浏览器中输入不同的URL，可进入相应界面
    - 当浏览器访问 http://localhost:8000?username=abc 时，如果abc是已经注册的用户，则显示abc的“详情”界面 其它情况均显示“注册”界面
    - “注册”界面点击“重置”，清空表单所有内容
    - “注册”界面点击“提交”，成功则跳转到对应用户的“详情”界面，不成功则回到注册界面，并显示错误原因；注意，请使用POST提交
+ 校验
    - 用户名6~18位英文字母、数字或下划线，必须以英文字母开头
    - 学号8位数字，不能以0开头
    - 电话11位数字，不能以0开头
    - 邮箱按照课程讲义中的规则校验
    - 校验发现错误时，要在界面上提示具体出错的原因
    - 用户名、学号、电话、邮箱均不可重复，重复时要在界面上显示具体重复的内容
+ 技术要求
    - 使用nodejs，并且必须直接实现服务器，也就是说不能使用express等框架
    - 可以不使用数据库保存数据，而使用文件保存
<!-- more -->

### **相关实现**
+ 实现代码 [Github](https://github.com/sysuKinthon/Web2.0/tree/master/Web2.0/node_register)
+ 实现效果
    - 注册页面
    ![效果图](http://7xncgn.com1.z0.glb.clouddn.com/16-2-27/97708366.jpg)
    - 注册并登录成功
    ![效果图](http://7xncgn.com1.z0.glb.clouddn.com/16-2-27/37960716.jpg)
### **经验所得**
+ 对文档中的引用对象的请求，对应报文的头部Content-Type的设置
    - 这里还需要了解[MIME](http://www.ruanyifeng.com/blog/2008/06/mime.html);在我们通过localhost:8000获取到注册页面(index.html)时，得到的页面还引用到了css和js文件(没有图片)，如图
    ![效果图](http://7xncgn.com1.z0.glb.clouddn.com/16-2-27/46877736.jpg)
    - 所以在浏览器解析这个html文件时，知道还需要index.css, jquery.js, index.js这三个外部对象，对这三个对象浏览器会向服务器发送请求，这个可以通过Chrome的开发者工具中的Network看出
    ![效果图](http://7xncgn.com1.z0.glb.clouddn.com/16-2-27/63291151.jpg)
    - 那么通过查看我们的请求报文，比如以index.css来说可以得到如下的信息
    ![效果图](http://7xncgn.com1.z0.glb.clouddn.com/16-2-27/67800812.jpg)
    - 这里主要想说下Content-Type,这个是MIME中的语句，我们通过在响应报文头中设置Content-Type这个属性来让浏览器知道其收到的文件的类型是什么，从而让浏览器知道如何解析其收到的文件，而不会导致将css文件当做html文件来解析，具体的设置方式如下代码所示
```javascript
var mimetype = {
    'txt': 'text/plain',
    'html': 'text/html',
    'css': 'text/css',
    'js':'text/javascript'
}
var server = http.createServer(function(request, response) {
    request.setEncoding("utf8");
    if(request.method === "GET") {
        var username = querystring.parse(url.parse(request.url).query).username;
        if(username !== undefined && queryUser(username)) {
            userInfoPage(response, getUser(username));
            return;
        }
        var pathname = path.parse(url.parse(request.url).pathname);
        var ext = pathname['ext'].substr(1);
        var base = pathname['base'];
        if(ext in mimetype) {
            response.writeHeader(200, {"Content-Type": mimetype[ext]});
            var data = fs.readFileSync('./'+base);
            response.write(data);
            response.end();
            return;
        }
        response.writeHeader(200, {"Content-Type": "text/html"}); 
        response.write(html);
        response.end();
    }
    if(request.method === "POST") {
        request.on('data', function(data) {
            var userInfo = querystring.parse(data);
            var error = "";
            if(counter(userInfo) === 1) {
                for(item in userInfo) {
                    error = validItem(response, userInfo[item], item);
                }
                errorJson = JSON.stringify(error);
                response.end(errorJson);
            } else {
                updateUser(userInfo);
                userInfoPage(response, userInfo);
            }
        });
    }
});
```
+ 如何使用ajax来实现post请求
    - 在要求中需要我们对于信息的唯一性进行判断，所以就需要在网页中发送post请求去将当前网页的内容与后台的数据进行对比，在实现中是利用了ajax的post进行请求，但是这里需要记住在ajax的post是异步的；
```javaScript
function isRepeatedForSubmit(target) {
    var name = target.name, value = target.value;
    var obj = {}, flag = false;
    obj[name] = value;
    //变为同步post请求，重要的一步。
    $.ajaxSetup({
        async: false
    });
    $.post("/", obj, function(data) {
        var error = $.parseJSON(data);
        if(error !== "") {
            flag = true;
        } else {
            flag = false;
        }
    });
    return flag;
}
```
    - 上面这个函数是每当我们填写完一个表单项目，对应的输入框失去焦点时，会进行的回调函数，也就是通过post请求向后台查询当前的信息是不是不重复的；如果没有设置ajax是同步的话，我们每次回调函数isRepeatedForSubmit返回的flag都是一开始设置的false，而不是根据post设置的；原因在于，我们的post是异步的，当我们isRepeatedForSubmit函数结束返回时，我们的post函数还在运行，也就是还没有结束，所以我们是不能得到其结果的。
    - 实例测试：为了测试异步，我们首先将submitHandler函数的最后加上event.preventDefault()，防止表单提交后跳转了页面，然后将isRepeatedForSubmit函数修改为
```javaScript
function isRepeatedForSubmit(target) {
    var name = target.name, value = target.value;
    var obj = {}, flag = false;
    obj[name] = value;
    //变为同步post请求，重要的一步。
/*    $.ajaxSetup({
        async: false
    });*/
    $.post("/", obj, function(data) {
        var error = $.parseJSON(data);
        if(error !== "") {
            flag = true;
        } else {
            flag = false;
        }
        console.log("inner:", flag);
    });
    console.log("outer:", flag);
    return flag;
}
```
    - 以下是在提交的表单重复的情况下函数的输出

![效果图](http://7xncgn.com1.z0.glb.clouddn.com/16-2-27/28521796.jpg)

    - 从上面可以看出是先输出outer：false四次后，，然后再输出四次Inner:true,并不是我们一开始设想的outer在inner输出后再输出，且outer为true,这就是因为我们四次的测试函数结束后，第一个post函数的回调函数还没有结束。
+ json的两个函数的使用和其返回的类型；
    - 在nodejs中服务器解析从客户端post过来的信息(是一个对象)，可以使用querystring模块中的parse函数，解析来出是一个对象；对应的服务器要返回信息给客户端时，我们需要对信息进行序列化(JSON.stringify())，使其可以在网络中传送；在客户端代码中我们使用JSON.parse()来解析得到我们需要的返回值
+ nodejs中的querystring模块与url模块
    - 利用这两个模块我们可以操作url，从其中得到我们需要的查询参数，例如假设我们的url为http://localhost:8000?username=kinthon,那么通过  querystring.parse(url.parse(request.url).query).username这个语句我们就可以得到kinthon这个参数值了。其中 request.url是/?usename=Kinthon;  url.parse(request.rul)得到了一个Url对象， url.parse(request.url).query获得得到的Url对象的parse，其值为'username=Kinthon'; querystring.parse(url.parse(request.url).query)通过字符串'username=Kinthon'得到一个对象这个对象为{username:'Kinthon'}; 