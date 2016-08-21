title: 纯JS实现fadeIn 和fadeOut
date: 2016-02-27 16:56:22
tags: Web
---
> css 相关透明度的设置方式
    filter:alpha(opacity=50);
    opacity: 0.5;

+ 思路： 主要的思路就是通过setTimeout函数来设置实时操作，通过每隔一段时间来减小/增加相关元素的透明度从而实现其淡入与淡出的效果
<!-- more -->

+ 代码如下
```javaScript
function setOpacity(elem, opacity) {
    /*
     * elem : The id of the element; 
     * opacity: The value of alpha, which is a decimals.
     */
    if(elem.style.filter) {   //IE
        elem.style.filter = 'alpha(opacity:' + opacity * 100 + ')';
    } else {
        elem.style.opacity = opacity;    
    }
}
/*
function getOpacity(elem) {
    return (elem.style.filter ? elem.style.filter)
}*/
function fadeIn(elem, speed) {
    /* 
     * elem, the id of the element;
     * speed, the speed for the fadeIn.(The value lower, the less time needs)
     * opacity, the target opacity will be reach, 0.0 to 1.0
     */
     elem.style.display = "block";
     setOpacity(elem, 0);

     var tempOpacity = 0;
     (function(){
         setOpacity(elem, tempOpacity);
         tempOpacity += 0.05;
         if(tempOpacity <= 1) {
             setTimeout(arguments.callee, speed);
             //tempOpacity += 0.05;
         }
     })();
}

function fadeOut(elem, speed) {
    /* 
     * elem, the id of the element;
     * speed, the speed for the fadeout;
     speed, the speed for the fadein.(The value lower, the less time needs);
     */
     elem.style.display="block";
     var tempOpacity = 1;
     (function(){
         setOpacity(elem, tempOpacity);
         tempOpacity -= 0.05;
         if(tempOpacity > 0) {
             setTimeout(arguments.callee, speed);
             console.log("why");
         } else {
             elem.style.display = "none"; //不可放在匿名函数外面会先执行。
         }
     })();
     //elem.style.display = "none";
}

function fadeTo(elem, speed, opacity){
    /* elem, the id of the element;
     * speed, the speed to of the fadeTo.(The value lower, the less time needs)
     * opacity, the opacity of the final result;
     */
     var tempOpacity = 0;
     elem.style.display = "block";
     (function(){
         setOpacity(elem, tempOpacity);
         tempOpacity += 0.05;
         if(tempOpacity <= opacity) {
             setTimeout(arguments.callee, speed);
         }
     })();
}
```
+ 这里需要有一个值得注意的，就是在设置定时修改透明度时，这里利用了JS的闭包性质。以图中fadeOut代码为例说明，在这个代码的运行过程中，在fadeOut函数中定义了一个内部的自执行匿名函数来实现定时减小透明度，在匿名函数内部，使用setTimeout来定时的调用匿名函数（arguments.callee,注意这里的arguments的作用域是匿名函数，不是fadeOut),这个方法看似递归调用了匿名函数，但是实际上解析器并不是这样处理的。当fadeOut函数触发时，运行的过程是：fadeOut函数入栈执行--->匿名函数入栈执行--->匿名函数出栈--->fadeOut函数出栈--->设置的时间到了，匿名函数入栈--->匿名函数出栈--->设置的时间到，匿名函数入栈……；从效率上来看，这样的过程是更好的，因为减小的函数栈的压力；同时也可以看到JS闭包的性质了，因为虽然定义tempOpacity的函数fadeOut出栈了，但是后来的匿名还是可以访问到这个变量。这主要是因为虽然fadeOut函数返回了，其执行环境的作用域链被销毁了， 但是其活动对象（由每个函数的arguments和其他命名参数来初始化得到的）仍然会留在内在中，因为匿名函数中有引用到这个对象（js的内存管理是使用垃圾回收机制的），所以匿名函数也就可以使用这个对象里面的变量了，只有当匿名函数被销毁时，其活动对象才会被销毁。
+ 参考网址
    - http://mrthink.net/js-fadeIn-fadeOut-fadeTo/