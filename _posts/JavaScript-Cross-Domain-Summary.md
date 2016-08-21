---
title: JavaScript跨域总结
date: 2016-7-20 20:22:33  
tags:
  - JavaScript
  - 跨域
categories: JavaScript
---

## 同源策略

同源策略阻止从一个源加载的文档(document)或脚本获取或设置另一个源加载的文档(document)的属性。简单地说就是要求动态内容（例如，JavaScript或者VBScript）只能阅读与之同源的那些`HTTP`应答和`cookies`，而不能阅读来自不同源的内容。
随着互联网的发展，"同源政策"越来越严格。目前，如果非同源，共有三种行为受到限制。
> （1） Cookie、LocalStorage 和 IndexDB 无法读取。
（2） DOM 无法获得。
（3） AJAX 请求不能发送。

<!-- more -->

## 什么是跨域?

两个页面只要协议(protocol)，端口(如果指定)，和主机三者中有任何一个不同，就被当作是跨域。 
下表给出了相对`http://www.niices.com/demo/index.html`同源检测的示例:

| URL                                      |  结果  |  原因   |
| :--------------------------------------- | :--: | :---: |
| http://www.niices.com/dir/other.html     |  成功  |       |
| http://www.niices.com/demo/inner/another.html |  成功  |       |
| https://www.niices.com/secure.html       |  失败  | 协议不同  |
| http://www.niices.com:81/demo/etc.html   |  失败  | 端口不同  |
| http://demo.niices.com/demo/other.html    |  失败  | 主机名不同 |

跨域并非浏览器限制了发起跨站请求，而是跨站请求可以正常发起，但是返回结果被浏览器拦截了。注意：有些浏览器不允许从HTTPS的域跨域访问HTTP，比如Chrome和Firefox，这些浏览器在请求还未发出的时候就会拦截请求，这是一个特例。

本文主要讨论如下五种跨域方法：
1.通过`JOSNP`解决跨域问题
2.`CORS`跨域资源共享
3.`document.domain`+`iframe`跨域
4.`window.name`+`iframe`实现的跨域数据传输
5.使用`HTML5`的`postMessage`方法

## 通过`JOSNP`解决跨域问题

通过日常的开发经验，我们可以得知。我们直接用`XMLHttpRequest`请求不同域上的数据时，是不可以的。而使用`<script>`、`<img>`、`<iframe>`、`<link>`等带有`src`标签都可以跨域加载资源,严格说这都是不符合同源要求的。

所以JSONP的原理就很简单了:
本地创建一个函数,并将函数名用`callback=functionName`的形式传递给服务器端。服务器端动态创建本地函数的调用并把我们需要的`JSON`数据的形式以参数的形式返回。

```js
<script type="text/javascript">
    function functionName(jsondata){
        //处理获得的json数据
    }
</script>
<script src="http://example.com/data.php?callback=functionName"></script>
```

`JQuery`封装的方法进行`JSONP`操作：

```js
<script type="text/javascript">
    $.getJSON('http://example.com/data.php?callback=?,function(jsondata)'){
        //处理获得的json数据
    });
</script>
```

这里的回调函数名可以用?代替，`JQuery`会自动帮你处理数据。

### `JSONP`优缺点

优点：它不像`XMLHttpRequest`对象实现的`Ajax`请求那样受到同源策略的限制；它的兼容性更好

缺点：由于是使用的`<script>`标签，所以它只能支持`GET`请求而不支持`POST`等其它类型的`HTTP`请求；

Tips：
> `Ajax`和`JSONP`这两种技术在调用方式上“看起来”很像，目的也一样，都是请求一个`url`，然后把服务器返回的数据进行处理，因此`JQuery`把`JSONP`作为`Ajax`的一种形式进行了封装；

> 但`Ajax`和`JSONP`其实本质上是不同的东西。`Ajax`的核心是通过`XmlHttpRequest`获取非本页内容，而`JSONP`的核心则是动态添加`<script>`标签来调用服务器提供的`js`脚本。

## `CORS`跨域资源共享

`CORS(Cross-Origin Resource Sharing)`定义一种跨域访问的机制，可以让`AJAX`实现跨域访问。

其基本原理就是使用自定义的`HTTP`头部允许浏览器和服务器相互了解对方，从而决定请求或响应成功与否。需要服务器端的支持，主要是通过设置`Access-Control-Allow-Origin`来进行。如果浏览器检测到相应的设置，就可以允许`Ajax`进行跨域的访问。

```
Header set Access-Control-Allow-Origin \* 
```

上面默认所有域都可以访问我们的服务器。

![](/images/2016/07/1.png)

安全起见我们可以限制域访问：

```
Access-Control-Allow-Origin: http://lover.niices.com
```

这样不是来自`http:lover.niices.com`域的请求就会报错。

![](/images/2016/07/2.png)

客户端代码如下：

```js
var xhr = new XMLHttpRequest();
xhr.open("GET", "http://demo.niices.com/xhr.php", true);
xhr.send();
xhr.onreadystatechange = function() {
	if (xhr.readyState == 4 && xhr.status == 200) {
	    console.log(xhr.responseText);
	}
}
```

将`Ajax`请求的`url`由相对路径变成了我们需要跨域访问的绝对路径。

服务器端代码如下：

```php
\<?php
$result = array ('a'=\>1,'b'=\>2,'c'=\>3,'d'=\>4,'e'=\>5);

header('content-type:application:json;charset=utf8');
header('Access-Control-Allow-Origin:\*');
header('Access-Control-Allow-Methods:POST');
header('Access-Control-Allow-Headers:x-requested-with,content-type');

echo json\_encode($result);
?\>
```

### `CROS`相比`JSONP`的优势

1、`JSONP`只能实现`GET`请求，而`CORS`支持所有类型的`HTTP`请求。

2、使用`CORS`，开发者可以使用普通的`XMLHttpRequest`发起请求和获得数据，比起`JSONP`有更好的错误处理。

3、`JSONP`主要被老的浏览器支持，它们往往不支持`CORS`，而绝大多数现代浏览器都已经支持了`CORS`。

## `document.domain`+`iframe`跨域

对于主域相同而子域不同的例子，可以通过设置document.domain的办法来解决。

不同的框架之间是可以获取window对象的，但却无法获取相应的属性和方法。比如，有一个页面，它的地址是`http://www.example.com/a.html`，在这个页面里面有一个`iframe`，它的src是`http://example.com/b.html`, 很显然，这个页面与它里面的`iframe`框架是不同域的，所以我们无法通过页面`a`里的`js`代码来获取`iframe`中的东西。

a.html

```js
<script type="text/javascript">
    function test(){
        var iframe = document.getElementById('￼ifame');
        var win = document.contentWindow;//可以获取到iframe里的window对象，但该window对象的属性和方法几乎是不可用的
        var doc = win.document;//这里获取不到iframe里的document对象
        var name = win.name;//这里同样获取不到window对象的name属性
    }
</script>
<iframe id = "iframe" src="http://example.com/b.html" onload = "test()"></iframe>
```

这时候我们只要把`http://www.example.com/a.html`和`http://example.com/b.html`这两个页面的`document.domain`都设成相同的域名就可以了。但要注意的是，`document.domain`的设置是有限制的，我们只能把`document.domain`设置成自身或更高一级的父域，且主域必须相同。

a.html

```js
<iframe id="iframe" src="http://demo.niices.com/b.html" onload = "test()"></iframe>
<script type="text/javascript">
    document.domain = 'niices.com';//设置成主域
    function test(){
        alert(window.frames["iframeSon"].document;//contentWindow 可取得子窗口的 window 对象
    }
</script>
```

b.html

```js
<script type="text/javascript">
    document.domain = 'niices.com';//在iframe载入这个页面也设置document.domain，使之与主页面的document.domain相同
    function test(){
        alert(parent.document.window);//contentWindow 可取得子窗口的 window 对象
    }
</script>
```


## `window.name`+`iframe`实现的跨域数据传输

`window`对象有个`name`属性，该属性有个特征：即在一个窗口(window)的生命周期内,窗口载入的所有的页面都是共享一个`window.name`的，每个页面对`window.name`都有读写的权限，`window.name`持久存在一个窗口载入过的所有页面中
有三个页面：
a.com/app.html：应用页面。
a.com/proxy.html：代理页面。一般是一个没有任何内容的html文件，需要和应用页面在同一域下。
b.com/data.html：数据页面。应用页面需要获取数据的页面。
基本原理：在应用页面动态创建`iframe`并将其`src`设置为数据页面，此时数据页面会将应用页面所需要的数据附加到`iframe`的`window.name`上，然后将`iframe`的`src`设置为代理页面，然后应用页面就可以拿到`iframe`中的数据（代理文件和应用页面在同一域下，所以可以相互通信）了。最后删除此`iframe`保证数据的安全。

### 基本步骤

1.在应用页面中创建一个iframe，把其src指向数据页面。
数据页面会把数据附加到这个iframe的window.name上，data.html代码如下：

```js
<script type="text/javascript">
    window.name = 'I was there!';    // 这里是要传输的数据，大小一般为2M，IE和firefox下可以大至32M左右
                                     // 数据格式可以自定义，如json、字符串
</script>
```

2.在应用页面中监听iframe的onload事件，当数据页面加载完毕后将iframe的src指向本地域的代理文件。此时应用页面就能获取所需的数据了。app.html部分代码如下：

```js
<script type="text/javascript">
    var state = 0, 
    iframe = document.createElement('iframe'),
    loadfn = function() {
        if (state === 1) {
            var data = iframe.contentWindow.name;    // 读取数据
            alert(data);    //弹出'I was there!'
        } else if (state === 0) {
            state = 1;
            iframe.contentWindow.location = "http://a.com/proxy.html";    // 设置的代理文件
        }  
    };
    iframe.src = 'http://b.com/data.html';
    if (iframe.attachEvent) {
        iframe.attachEvent('onload', loadfn);
    } else {
        iframe.onload  = loadfn;
    }
    document.body.appendChild(iframe);
</script>
```

3.获取数据以后销毁这个iframe，释放内存；这也保证数据安全。
```js
<script type="text/javascript">
    iframe.contentWindow.document.write('');
    iframe.contentWindow.close();
    document.body.removeChild(iframe);
</script>
```

## 使用`HTML5`的`postMessage`方法

### 发送消息的`postMessage`方法：

```js
otherWindow.postMessage(message, targetOrigin);
```

> otherWindow其他窗口的一个引用，比如`iframe`的`contentWindow`属性、执行`window.open`返回的窗口对象、或者是命名过或数值索引的`window.frames`。
> message将要发送到其他`window`的数据，一般用`JSON`。
> targetOrigin
> 是限定消息接收范围，不限制请使用 ‘\*’

### 接受数据的`message`事件

```js
var onmessage = function (event) {
	var data = event.data;
	var origin = event.origin;
	var source = event.source;
	// do something
};
if (typeof window.addEventListener != 'undefined') {
	window.addEventListener('message', onmessage, false);
} else if (typeof window.attachEvent != 'undefined') {
	//for ie
	window.attachEvent('onmessage', onmessage);
}
```

回调函数第一个参数接收一个`event`对象，有三个常用属性：

> data:  消息
> origin:  消息来源地址
> source:  源 DOMWindow 对象

### 举个栗子

首先在`a.html`页面获取`b.html`页面的引用，并在`b.html`页面加载完成时向其发送数据，同时监听有没有从`b.html`页面返回的数据。
http://blog.niices.com/a.html

```js
<iframe id="iframe" src="http://demo.niices.com/b.html"></iframe>
<script>
// 获取b.html页面的引用。
var iframe=document.getElementById("iframe");
// iframe加载完成后向其发送数据
iframe.onload=function(){
  iframe.contentWindow.postMessage({"age":21},"http://demo.niices.com");
}
// 监听返回的数据
function receiveMessage(event) {
    if (event.origin !== "http://demo.niices.com")          return;
    console.log(event.data)
}
window.addEventListener('message', receiveMessage, false);
</script>
```

然后在b页面接受传递的消息，并在得到数据后返回一个确认的消息
http://blog.niices.com/b.html

```js
<script>
function receiveMessage(event)
{
  if (event.origin !== "http://blog.niices.com")
    return;
  console.log(data.age);
  // 接收到数据后返回一个确认消息
  event.source.postMessage("I got the message!",event.origin);
}
window.addEventListener("message", receiveMessage, false);
</script>
```

如果能看到下面的结果证明我们跨域成功了！
![](/images/2016/08/1.png)


## 参考来源

[HTTP访问控制(CORS)-MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS)
[JavaScript跨域总结与解决办法](http://www.cnblogs.com/rainman/archive/2011/02/20/1959325.html)
[详解js跨域问题](https://segmentfault.com/a/1190000000718840#articleHeader6)
[window.postMessage-MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/postMessage)
[使用window.postMessage实现跨域通信](http://www.css88.com/archives/4720)
