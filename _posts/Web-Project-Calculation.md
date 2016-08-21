title: JS 之简单计算器
date: 2016-02-27 16:36:07
tags:
- Web
- Project
---

### **要求**
+ 响应用户对数字和算术操作符按钮的操作，记录并显示用户通过按钮输入的算术表达式（50分）响应用户功能按钮的操作
+ 用户按下“←”按钮，删除当前算术表达式最后一个字符，并更新显示
+ 用户按下“CE”按钮，清除当前算术表达式（20分）用户按下“=”按钮，计算当前表达式的结果并显示
+ 如果，算术表达式非法，弹出警告框提醒用户，并终止计算
<!-- more -->

### **实现**
+ 效果图
![Calculation](http://7xncgn.com1.z0.glb.clouddn.com/16-2-27/637943.jpg)
+ 实现代码 [Github](https://github.com/sysuKinthon/Web2.0/tree/master/Web2.0/calculator)

### **经验所得**
+ 其实代码实现不难，而且用了已经不大推崇的eval函数，这个函数会带来代码注入的问题
+ 实现过程中主要想说的是一种思维，因为之前一直都不怎么接触事件处理机制，一直都是用c,c++,java来打代码，所以在写这个的时候有一个致命的思维误区:
```javaScript
    //if input the symbol, the reslut should be clear;
    for(i = 0; i < symbolList.length; ++i) {
        symbolList[i].onclick = function(event) {  //不能使用i来索引对象，因为当Onclick事件发生时，i已经都是20了。
            if(flag == true) {
                expression.value = "";
                flag = false;
            }
            var content = event.target.innerText;
            expression.value = expression.value + content;
        }
    }
```
一开始在for循环里面，一上手就想用sybmolList[i]来引用innerText;也就是：
```javaScript
var content = event.target.innerText;
换成
var content = symbolList[i].innerText;
```
发现我一点击事件就出现越界行为，输出查看i,i的值都是同一个数，这个数决定于window.onload回调函数结束后的i值。要清楚，这是闭包导致的，当我们的DOM加装完毕后，window.onload事件就发动了，并调用事件处理；好像一切都没有问题，但是想想看，当我们点击事件，并触发了symbolList中的对象时，回调了我们设置的函数，此时它引用i的值的话，必然是symbolList.length,因为onload事件已经执行了，但由于闭包，如果我们的点击函数中有引用外部的变量的话，是可以访问到的，在javascript中是采用了垃圾处理机制，闭包包含了外部的活动对象，所以这个对象还没有被销毁，可以被引用。所以我们在程序中是不能直接调用 i来索引事件源的，要使用event.target
> 这里要提一下javaScript的事件回调机制，javaScript的回调函数是放在事件队列中的，而且只有当当前的函数栈为空时，才会才调度事件队列中的函数进函数栈中执行

+ 使用中缀表达式转后缀表达式来实现
    - 毕竟利用eval来实现计算会导致一些意想不到的问题，所以就用算法实现了下中缀表达式的计算:参考
    - 关于考虑运算符优先级的问题，也就是什么时候栈里面的运算符应该出栈，主要考虑一个方面就可以了，就是运算符的栈始终要保持栈顶的元素优先级最高，相同优先级的后面进入的优先级高，同时需要考虑对于")"来说，当在遇到"("时，它的优先级是最高的，遇到后，就是最低的了(也就是不要入栈）。
```javaScript
// calculator the postfix expression
function calculate(postfix) {
    var i, item, result;
    var num = [];
    for(i = 0; i < postfix.length; i++) {
        item = parseFloat(postfix[i]);
        if(!isNaN(item)) {
            num.push(item);
        } else {
            second = num.pop();
            first = num.pop();
            switch(postfix[i]) {
                case '+':
                    result = first + second;
                    num.push(result);
                    break;
                case '-':
                    result = first - second;
                    num.push(result);
                    break;
                case '/':
                    result = first / second;
                    num.push(result);
                    break;
                case '*':
                    result = first * second;
                    num.push(result);
                    break;
            }
            //console.log(result);
        }
    }
    result = num.pop();
    result = parseFloat(result.toFixed(8), 10);
    return result;
}


// translate the infix expression to the postfix expression
function toPostfix(infix) {
    //split the input to the token
    var regex = /(\(|\)|\/|\*|\-|\+)/;
    var array= infix.split(regex);
    var i;
    var postfix = [];
    var symbol = [];
    // remove the empty item
    for(i = 0; i < array.length; i++) {
        if(array[i] == "")
            array.splice(i, 1);
    }

    for(i = 0; i < array.length; i++) {
        var item = array[i];
        if(!isNaN(parseFloat(item))) {
            postfix.push(item);
        } else {
            while(symbol.length) {
                if(compare(item, symbol[symbol.length-1])) { //if the item is greater
                    break;
                } else {
                    postfix.push(symbol.pop());
                    /*if(postfix[postfix.length-1] == "(")
                        console.log(1);*/
                }
            }
            if(item == ")") {
                symbol.pop(); //pop "("
            } else { 
                symbol.push(item);
            }
        }
    }
    while(symbol.length) {
        if(symbol) {
            postfix.push(symbol.pop());
        }
    }
    return postfix;
}

function compare(item, top) {
    if(item.match(/\)/) && top.match(/\(/))
        return true;
    if(item.match(/\(/) || top.match(/\(/)) {
        return true;
    }
    if(item.match(/\*|\//) && top.match(/\+|\-/)) {
        return true;
    }
    //console.log(item);
    return false;
}
```