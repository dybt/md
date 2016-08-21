---
layout: post
title: "如何让你的iOS7应用支持键盘快捷键"
date: 2013-12-26 22:50:35 +0800
comments: true
categories: iOS
---

最近买了个罗技的光能无线键盘（[K760](http://www.amazon.cn/Logitech-%E7%BD%97%E6%8A%80-K760-%E5%A4%AA%E9%98%B3%E8%83%BD%E9%94%AE%E7%9B%98/dp/B008KW90OM/ref=sr_1_cc_1?s=aps&ie=UTF8&qid=1388078016&sr=1-1-catcorr&keywords=k760)）用来连接我的iOS设备，但是发现iOS所支持的快捷键太少了（特别是iPhone），而iPad也仅有那几款应用支持那可怜的几个快捷键。这使得我感觉iOS设备用键盘如同鸡肋。。。因为打字什么的还是要手点。

记得之前了解到iOS 7的UIResponder新增了keyCommands方法，于是今天就研究了一下API文档并且测试了一下，感觉真是太棒了~

下面介绍一下如何让应用支持外接键盘的快捷键，其实很简单！

<!--more-->

首先要确保在需要实现键盘快捷键的ViewController中加入如下代码：

```objc
- (BOOL)canBecomeFirstResponder {
    return YES;
}
```

然后就是重写`UIViewController`的`- (NSArray *)keyCommands`方法：

```objc
- (NSArray *)keyCommands {
    return @[[UIKeyCommand keyCommandWithInput:UIKeyInputEscape modifierFlags:kNilOptions action:@selector(escapeKeyPressed:)]];
}
```

最后一步就是实现`UIKeyCommand`的`action`：

```objc
- (void)escapeKeyPressed:(UIKeyCommand *)keyCommand {
    UIAlertView *alertView = [[UIAlertView alloc] initWithTitle:@"UIKeyCommand Demo" message:[NSString stringWithFormat:@"%@ pressed", keyCommand.input] delegate:nil cancelButtonTitle:@"Ok" otherButtonTitles:nil];
    [alertView show];
}
```

现在试一下运行app，然后按一下`esc`键（模拟器下可以使用电脑的键盘），看一下效果~，怎么样？很棒吧！！！

**参考文档：**

- [UIResponder的keyCommands](https://developer.apple.com/Library/ios/documentation/UIKit/Reference/UIResponder_Class/Reference/Reference.html#jumpTo_5) [![Dash Icon][dash-icon]](dash://UIResponder%20keyCommands)

- [UIKeyCommand](https://developer.apple.com/Library/ios/documentation/UIKit/Reference/UIKeyCommand_class/Reference/Reference.html#//apple_ref/occ/cl/UIKeyCommand) [![Dash Icon][dash-icon]](dash://UIKeyCommand)

[dash-icon]: http://kapeli.com/dash_resources/dashHR.png