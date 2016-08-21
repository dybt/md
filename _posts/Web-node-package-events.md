title: Node events包源码阅读
date: 2016-07-09 18:59:19
tags:
- Web
- Node
---
### 预备
<!-- more -->
+ events的源码 [github](https://github.com/Gozala/events)
+ events的简单使用
```
var events = require('events')

var eventEmitter = new events()
//或 var eventEmitter = new events.EventEmitter()

function listen1(msg) { 
  console.log('listen1');
}

eventEmitter.on('connection', listen1)

eventEmitter.on('connection', function() {
  console.log('listen2');
})

eventEmitter.emit('connection','fdaf')

eventEmitter.on('error', function(msg) {
  console.log(msg)
})

eventEmitter.emit('error', new Error('kaka'))

eventEmitter.once('once', listen1)

eventEmitter.emit('once')
eventEmitter.emit('once')
```
+ events的实现的设计模式主要是观察者模式

### 阅读体会
+ 代码中的函数逻辑都比较清晰，先进行参数判断，是不是符合函数要求，然后再进行对应的逻辑处理
+ 针对once类型的处理，因为这种类型是只能触发一次的，源码里面的实现方式是，用一个`g`函数来间接运行函数，并使用闭包的特性利用fired来标志函数是否已经运行过；
