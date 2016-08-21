---
layout: post
title: "在Swift中创建完美的单例模式"
date: 2014-10-15 01:17:53 +0800
comments: true
categories: Swift
---

`Swift` 发布没多久学了一点 `Swift` 之后我就想过一个问题：`Objective-C` 中的单例在 `Swift` 中怎么实现呢？后面也搜过一些相关的资料，比如  `StackOverflow` 上有个问题 [dispatch_once singleton model in swift](http://stackoverflow.com/questions/24024549/dispatch-once-singleton-model-in-swift) ，答案中给了几种实现方法，但我总觉得很不完美，主要有以下两点不足（个人意见）：
<!-- more -->
1. 单例是通过类的属性获取的（ `Objective-C` 中通过类方法），这让人觉得很别扭
2. 主要问题是没有规避通过构造函数创建多个实例

当然我自己的实现中用了类方法而不是类属性，但是第二个问题仍然存在！昨天又搜到一篇文章 [singleton pattern in swift](http://vperi.com/2014/06/05/singleton-pattern-in-swift/) ，虽然第一个问题没有解决但是解决了第二个问题，后来经过我的改造之后实现了如下我自己觉得很完美的实现方法：

```swift
class SomeClass {
    private struct SharedStruct {
        static var dispatch_once_token: dispatch_once_t = 0
        static var sharedInstance: SomeClass? = nil
    }
    
    private class var sharedInstance: SomeClass {
        dispatch_once(&SharedStruct.dispatch_once_token, {
            SharedStruct.sharedInstance = SomeClass()
        })
        
        return SharedStruct.sharedInstance!
    }
    
    class func sharedInstance() -> SomeClass {
        return sharedInstance
    }
    
    /*
    init() {
        assert(sharedStruct.sharedInstance == nil, "Singleton already initialized!")
    }
    */
    // 这样会更好
    private init() {
        // do something.
    }
}
```

以上实现如果有什么不好的地方希望大神们批评、指出，另外如果有更好的方法希望大神们分享。

