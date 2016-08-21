title: Web HTML 问题收集
date: 2016-08-09 20:44:19
tags:
- Web
- HTML
---
### 前端html编码转义
+ 问题
    - 需要在前端页面中呈现html代码
    - 如果是少量代码的话，我们可以直接使用html[转义字符](http://tool.oschina.net/commons?type=2)进行转换，比如`<`对应了`&lt;`,目前的情况是有一整个文件需要你呈现
<!-- more -->
+ 知识 
    - `<` `>`等符号已经用来表示 HTML TAG，因此不能直接当作文本中的符号来使用，但在时需要在HTML页面中使用这些符号，所以这些就需要使用它的转义字符或是使用它们的unicode编码
    - ASCII码一共规定了256个字符， 有些字符在ASCII字符集中没有定义， 比如说某些希腊字母，因此需要使用转义字符来表示 [对照](http://tool.oschina.net/commons?type=2)
    - 在string中有charCodeAt() 方法可返回指定位置的字符的 Unicode 编码
    - [字符编码](http://www.ruanyifeng.com/blog/2007/10/ascii_unicode_and_utf-8.html), [查看工具](http://tool.oschina.net/commons?type=4)
+ 解决
    - 解决的方式当然就是使用进行字符替换咯
    - 以下代码来自 [github](https://gist.github.com/CatTail/4174511)
```
 var decodeHtmlEntity = function(str) {
  return str.replace(/&#(\d+);/g, function(match, dec) {
    return String.fromCharCode(dec);
  });
};

var encodeHtmlEntity = function(str) {
  var buf = [];
  for (var i=str.length-1;i>=0;i--) {
    buf.unshift(['&#', str[i].charCodeAt(), ';'].join(''));
  }
  return buf.join('');
};

var tag = '<div>just change</div>'
encodeHtmlEntity(tag);

```

### a元素的嵌套
+ a元素是不能存在嵌套关系的，不然会提前终止第一个a元素，使得两个元素并行，可以自行测试一下代码`  <a>fdaf<a href="">fda</a></a>`

