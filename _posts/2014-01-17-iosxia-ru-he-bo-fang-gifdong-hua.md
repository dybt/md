---
layout: post
title: "iOS下如何播放GIF动画"
date: 2014-01-17 22:19:39 +0800
comments: true
categories: iOS 
---

几个月前无意中看到[StackOverFlow](http://stackoverflow.com)上的一个帖子[How to get suitable CGImage from combined TIFF for display scale](http://stackoverflow.com/questions/14458299/how-to-get-suitable-cgimage-from-combined-tiff-for-display-scale)了解到了[ImageIO](https://developer.apple.com/library/ios/documentation/graphicsimaging/Reference/ImageIORefCollection/_index.html)这个框架，看到它可以读取[TIFF][TIFF]中的单张图片，当时就想能不能读取[GIF][GIF]中的图片呢？后来进一步研究后发现答案是肯定的。后来就想哪天细研究一下吧，顺便写篇博客记录一下。没想到我是这么的懒~，昨天才终于研究好了顺便写了个项目[WYZGIFAnimationView](https://github.com/azone/WYZGIFAnimationView)开源到了[Github][github]上。

<!-- more -->

[GIF][GIF]动画大家应该都知道，但是在iOS中却不能直接播放它，只能显示它的第一帧画面。iOS中可以使用[UIImageView](https://developer.apple.com/library/ios/documentation/uikit/reference/UIImageView_Class/Reference/Reference.html)并通过其[animationImages](https://developer.apple.com/library/ios/documentation/uikit/reference/UIImageView_Class/Reference/Reference.html#//apple_ref/occ/instp/UIImageView/animationImages)、[animationDuration](https://developer.apple.com/library/ios/documentation/uikit/reference/UIImageView_Class/Reference/Reference.html#//apple_ref/occ/instp/UIImageView/animationDuration)和[animationRepeatCount](https://developer.apple.com/library/ios/documentation/uikit/reference/UIImageView_Class/Reference/Reference.html#//apple_ref/occ/instp/UIImageView/animationRepeatCount)等属性来播放连续图片的动画。[UIImage](https://developer.apple.com/library/ios/documentation/uikit/reference/UIImage_Class/Reference/Reference.html)在iOS 5.0之后增加了[+ animatedImageNamed:duration:](https://developer.apple.com/library/ios/documentation/uikit/reference/UIImage_Class/Reference/Reference.html#//apple_ref/occ/clm/UIImage/animatedImageNamed:duration:)和[+ UIImage animatedImageWithImages:duration:](https://developer.apple.com/library/ios/documentation/uikit/reference/UIImage_Class/Reference/Reference.html#//apple_ref/occ/clm/UIImage/animatedImageWithImages:duration:)等方法，通过这些方法创建的[UIImage](https://developer.apple.com/library/ios/documentation/uikit/reference/UIImage_Class/Reference/Reference.html)对象赋值给[UIImageView](https://developer.apple.com/library/ios/documentation/uikit/reference/UIImageView_Class/Reference/Reference.html)的[image](https://developer.apple.com/library/ios/documentation/uikit/reference/UIImageView_Class/Reference/Reference.html#//apple_ref/occ/instp/UIImageView/image)属性，同样也可以播放动画。

我们如果想播放[GIF][GIF]动画，其实要做的就是将[GIF][GIF]动画中的每一帧拿出来转化成UIImage，放到一个数组，然后在通过读取[GIF][GIF]每一帧的延时时间并累加得出一个总时长，做完以上两步之后在通过上面的方法就可以播放[GIF][GIF]动画了。但是通过这种方式创建的动画有一个缺点，因为[GIF][GIF]动画很有可能不是匀速动画，它的每一帧延时可能都会不同。于是[WYZGIFAnimationView](https://github.com/azone/WYZGIFAnimationView)诞生了，说到这里突然想到[WYZGIFAnimationView](https://github.com/azone/WYZGIFAnimationView)还没有支持这一点，我要赶紧修正一下啦~


[github]: https://github.com
[TIFF]: http://en.wikipedia.org/wiki/Tagged_Image_File_Format
[GIF]: http://en.wikipedia.org/wiki/GIF

