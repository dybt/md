---
title: Swift 下 OpenGL 截图容易导致的内存泄露
date: 2016-03-11 10:38:57
tags:
- OpenGL
- Screenshots
- EAGLView
categories: Swift
---

由于业务需求，需要对两个（现在是三个了）中的一个 OpenGL 渲染的视图进行截图并增加模糊效果。开始用传统的方法进行截图，但是失败了（截取的是黑屏还是空白，忘记了。。。），后来意识到这个和一般的视图应该是不一样的，就到  Google 上搜了一下。由于没怎么玩过，又不怎么懂，就直接翻译成 `Swift` 了。开始是没发现什么问题的，都是但是到后来发现截图总是其中一个的（看那个先出来，基本上不是想要的那个），而不是想要的那个。

仔细研究后发现，OpenGL 的截图是根据 RenderBuffer 来截取的，由于没有指定 RenderBuffer 所以截取的图只是其中某一个。知道了这个也没用，由于这个 View 用的是第三方库，所以不知道这个 View 的 RenderBuffer。。。后果经过 Debug，发现这个 View 有个 `_colorRenderBuffer` 属性，这就好办啦~。经过改造后代码如下：

<!-- more -->

```swift
func snapshotForEAGLView(glView: UIView) -> UIImage {
   var width: GLint = 0, height: GLint = 0
   let scale: CGFloat = UIScreen.mainScreen().scale
   let prevGLContext = EAGLContext.currentContext()
   // glView 有个 _colorRenderBuffer 属性(由于 glView 是第三方库的视图，所以只能通过这种方法来获取 _colorRenderBuffer)，需要将它与 GL_RENDERBUFFER 绑定，否则出现的截图可能是另外一个 OpenGL 视图
   if let colorRenderBuffer = glView.valueForKeyPath("_colorRenderBuffer") as? Int {
       glBindRenderbuffer(GLenum(GL_RENDERBUFFER), GLuint(colorRenderBuffer))

       glGetRenderbufferParameteriv(GLenum(GL_RENDERBUFFER), GLenum(GL_RENDERBUFFER_WIDTH), &width)
       glGetRenderbufferParameteriv(GLenum(GL_RENDERBUFFER), GLenum(GL_RENDERBUFFER_HEIGHT), &height)

       // glView 有个 _context 属性，需要将它设置成当前的 EAGLContext，并且执行 presentRenderbuffer 方法用来把 CAEAGLLayer 渲染出来
       if let glContext = glView.valueForKeyPath("_context") as? EAGLContext {
           EAGLContext.setCurrentContext(glContext)
           glContext.presentRenderbuffer(Int(GL_RENDERBUFFER)) // 显示当前 view 的 CAEAGLLayer
       }
   } else {
       width = GLint(glView.bounds.width * scale)
       height = GLint(glView.bounds.height * scale)
   }

   let dataLength = width * height * 4
   let imageBytes = UnsafeMutablePointer<GLubyte>.alloc(Int(dataLength))
   // 将图像信息存入内存
   glPixelStorei(GLenum(GL_PACK_ALIGNMENT), 4)
   // 读取图像的像素信息并写入到 imageBytes
   glReadPixels(0, 0, GLsizei(width), GLsizei(height), GLenum(GL_RGBA), GLenum(GL_UNSIGNED_BYTE), imageBytes)

   let dataProvider = CGDataProviderCreateWithData(nil, imageBytes, Int(dataLength), nil)

   EAGLContext.setCurrentContext(prevGLContext)

   let colorSpace = CGColorSpaceCreateDeviceRGB()
   if let imageRef = CGImageCreate(Int(width), Int(height), 8, 32, Int(width) * 4, colorSpace, [CGBitmapInfo.ByteOrder32Big, CGBitmapInfo(rawValue: CGImageAlphaInfo.PremultipliedLast.rawValue)], dataProvider, nil, true, CGColorRenderingIntent.RenderingIntentDefault) {
       return UIImage(CGImage: imageRef, scale: scale, orientation: .DownMirrored)
   }
   
   return UIImage()
}
```

很棒！但是很快又发现了问题，挂时间稍长就会崩溃（这个在之前也是有的，只是精力全放在另外一个问题上了）！经过调试后发现有很严重的内存泄露问题！！！从代码可以很容易看出是由于 `imageBytes` 没有释放的原因，于是在 `let dataProvider = CGDataProviderCreateWithData(nil, imageBytes, Int(dataLength), nil)` 下面加了两行代码来释放内存：

```swift
imageBytes.destroy()
imageBytes.dealloc(Int(dataLength))
```

这样做虽然内存释放掉了，但是又出问题了。。。截的图不见了，调试发现图是空的。经过分析得知由于 `UIImage`、`dataProvider` 和 `imageRef` 用的是一块内存，所以上面两行代码等于把 UIImage 的内存也释放掉了。

现在要做的就是把 `imageBytes` 拷一份出来，这样再释放 `imageBytes` 就不会有影响了。通过查文档得知 `CGDataProviderRef` 可以通过 `CGDataProviderCreateWithCFData` 来创建，`CFDataRef` 又可以通过 `CFDataCreate` 来创建，通过 `CFDataCreate` 的官方文档得知是通过拷贝指定的缓冲字节来创建 `CFData` 的：

> Creates an immutable CFData object using data copied from a specified byte buffer.

虽然饶了一点，但是也算是找到好方法了，只要把 `let dataProvider = CGDataProviderCreateWithData(nil, imageBytes, Int(dataLength), nil)` 改成下面的代码就解决啦！

```swift
let data = CFDataCreate(nil, imageBytes, CFIndex(dataLength))
let dataProvider = CGDataProviderCreateWithCFData(data)
imageBytes.destroy()
imageBytes.dealloc(Int(dataLength))
```

最终代码：

```swift
func snapshotForEAGLView(glView: UIView) -> UIImage {
   var width: GLint = 0, height: GLint = 0
   let scale: CGFloat = UIScreen.mainScreen().scale
   let prevGLContext = EAGLContext.currentContext()
   // glView 有个 _colorRenderBuffer 属性(由于 glView 是第三方库的视图，所以只能通过这种方法来获取 _colorRenderBuffer)，需要将它与 GL_RENDERBUFFER 绑定，否则出现的截图可能是另外一个 OpenGL 视图
   if let colorRenderBuffer = glView.valueForKeyPath("_colorRenderBuffer") as? Int {
       glBindRenderbuffer(GLenum(GL_RENDERBUFFER), GLuint(colorRenderBuffer))

       glGetRenderbufferParameteriv(GLenum(GL_RENDERBUFFER), GLenum(GL_RENDERBUFFER_WIDTH), &width)
       glGetRenderbufferParameteriv(GLenum(GL_RENDERBUFFER), GLenum(GL_RENDERBUFFER_HEIGHT), &height)

       // glView 有个 _context 属性，需要将它设置成当前的 EAGLContext，并且执行 presentRenderbuffer 方法用来把 CAEAGLLayer 渲染出来
       if let glContext = glView.valueForKeyPath("_context") as? EAGLContext {
           EAGLContext.setCurrentContext(glContext)
           glContext.presentRenderbuffer(Int(GL_RENDERBUFFER)) // 显示当前 view 的 CAEAGLLayer
       }
   } else {
       width = GLint(glView.bounds.width * scale)
       height = GLint(glView.bounds.height * scale)
   }

   let dataLength = width * height * 4
   let imageBytes = UnsafeMutablePointer<GLubyte>.alloc(Int(dataLength))
   // 将图像信息存入内存
   glPixelStorei(GLenum(GL_PACK_ALIGNMENT), 4)
   // 读取图像的像素信息并写入到 imageBytes
   glReadPixels(0, 0, GLsizei(width), GLsizei(height), GLenum(GL_RGBA), GLenum(GL_UNSIGNED_BYTE), imageBytes)

   let data = CFDataCreate(nil, imageBytes, CFIndex(dataLength))
   let dataProvider = CGDataProviderCreateWithCFData(data)
   imageBytes.destroy()
   imageBytes.dealloc(Int(dataLength))

   EAGLContext.setCurrentContext(prevGLContext)

   let colorSpace = CGColorSpaceCreateDeviceRGB()
   if let imageRef = CGImageCreate(Int(width), Int(height), 8, 32, Int(width) * 4, colorSpace, [CGBitmapInfo.ByteOrder32Big, CGBitmapInfo(rawValue: CGImageAlphaInfo.PremultipliedLast.rawValue)], dataProvider, nil, true, CGColorRenderingIntent.RenderingIntentDefault) {
       return UIImage(CGImage: imageRef, scale: scale, orientation: .DownMirrored)
   }
   
   return UIImage()
}
```

