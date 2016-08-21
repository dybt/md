title: 前端JS种种问题收集
date: 2016-02-27 17:07:57
tags:
- Web
- JS
---

### 问题1
+ 如果需要在文本区中当最大长度达到了，想显示后面输入的内容时，可以利用如下函数设定scrollLeft的值
```javaScript
function srcollToEnd() {
    expression.scrollLeft = expression.scrollWidth - expression.clientWidth;
}
```

### 问题2
+ 当我们双次或是多次点击双击某些div/span/p的时候不选定当前内容，时可能导致无法选定当前对象的情况，而出现选定了一大片，形如:
![效果图](http://7xncgn.com1.z0.glb.clouddn.com/16-2-27/2725898.jpg)
可以使用如下的函数消除这种问题
```javaScript
document.onselectstart = function() {
    return false;
}
```
<!-- more -->

### 问题3
+ 有时我们需要一次给一堆DOM元素设置事件，需要用到循环，此时由于事件函数是回调的，而且由于闭包的特性，会导致一些奇怪的问题(比如自增变量i都是一个同一个数)，原因主要在于回调函数的发生与包含该函数的函数不是同步的，此时的做法可以是利用闭包(自执行)或是利用event.示例如下:
![效果图](http://7xncgn.com1.z0.glb.clouddn.com/16-2-27/49918078.jpg)
或是
```javaScript
var symbolList = document.getElementsByClassName("symbol");

for(i = 0; i < symbolList.length; ++i) {
    symbolList[i].onclick = function(event) {  
        var    content = event.target.innerText;
    }
}
```

### 问题4
+ 闭包中的this丢失问题
    - 问题如下
    ![效果图](http://7xncgn.com1.z0.glb.clouddn.com/16-2-27/54716940.jpg)
    - 修改：
    ![效果图](http://7xncgn.com1.z0.glb.clouddn.com/16-2-27/92490023.jpg)

### 问题5 
+ 批量的增加DOM元素;
    - 关于js 每一次append之后导致整个dom tree 的重新计算排布，以及在页面的重新渲染(虽然很多浏览器会在这里进行优化），导致页面加载有明显卡顿，影响用户体能; 可以使用文档片段来解决，将文档中的节点添加到文档片段中，就会从文档树中移除该节点，添加到文档片段中的新节点同样也不属于文档树，可以通过appendChild(), insertBefore将文档片段中的内容添加中文档中，此时文档片段中的子节点被删除并移到文档中
    ```javaScript
function createMoles (moles) {
    var i;
    var fragment = document.createDocumentFragment();
    var newMole = null;
    for(i = 0; i < 60; i++) {
        newMole = document.createElement("input");
        newMole.setAttribute("type", "radio");
        newMole.setAttribute("name", "mole");
        fragment.appendChild(newMole);
    }
    moles.appendChild(fragment);
}
    ```

### 问题6
> 关于自执行函数前面需要分号的缘由

+ 有如下代码
    -  代码
    ```
 var a = {}
 (function(a){})(a)
    ```

    - 如果运行的话会有这样的错误`VM666:3 Uncaught TypeError: (intermediate value) is not a function(…)`; 原因在于要给var a = {}后面加上`;` 
    - 原因在于，立即调用的函数表达式本身是一个表达式，如果你直接放在了var a = {}后面的话，会出现的情况是，解析器在解析时，将立即执行的函数表达式解析执行后得到一个值(无return就是返回undefined)，如果它的前面不是一个分号的话，那么就当做它前面有一个函数，将自己的返回值做为参数给传入函数，但是发现`{}`本身并不是一个函数，所以报错。
    - 基于上述的机制，可以书写如下的代码
    ```
    function test(arg) {return arg};
    var a = test (function(){return "a"})() //执行后a = "a"
    ```

