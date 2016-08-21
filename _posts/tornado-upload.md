title: tornado 上传文件及修改
date: 2015-05-05 13:32:01
categories: tornado
tags: [tornado,]
---
实现**tornado**上传图片并对图片大小进行修改
### 图片的上传
上传图片使用了表单提交, 下面是**html**部分, `enctype="multipart/form-data"`表示不对字节进行编码，上传文件类型时需指定. input标签的 `type="file"` 指定上传类型.
```html
<form action="/" enctype="multipart/form-data" method="post">
    <input type="file" name="headimg">
</form>
```
下面是tornado接受文件的部分
```python
class UploadHandler(BaseHandler):
    def post(self):
        # 这部分就是上传的文件,想要查看更多可以print self.request看看
        # 该文件返回一个元素为字典的列表
        imgfile = self.request.files.get('headimg')
        for img in imgfile:
            # img有三个键值对可以通过img.keys()查看
            # 分别是 'filename', 'body', 'content_type' 很明显对应着文件名,内容(二进制)和文件类型
            with open('./static/uploads/' + img['filename'], 'wb') as f:
                # 文件内容保存 到'/static/uploads/{{filename}}'
                f.write(f['body'])
```
这样就可以通过`/static/uploads/文件名`的形式访问

### 对图片大小进行修改
下面将写对图片进行的resize
```python
class UploadHandler(BaseHandler):
    @tornado.web.authenticated
    def post(self):
        # 应该写到上面, 为了显示写到了函数内
        import time
        # PIL 是 python中对图片进行操作的模块, 感兴趣可以去看一下
        from PIL import Image
        # 可以想文件一样使用, 只是存放在内存
        from cStringIO import StringIO

        # 判断上传文件大小
        size = int(self.request.headers.get('Content-Length'))
        if size / 1000.0 > 2000:
            self.write("上传图片不能大于2M.")
        imgfile = self.request.files.get('headimg')
        for img in imgfile:
            # 对文件进行重命名
            name = str(time.strftime('%Y%m%d%'), time.localtime())\
                    + '_' + self.current_user + '_headimg.png'

            with open('./static/uploads/' + name, 'wb') as f:
                # image有多种打开方式，一种是 Image.open('xx.png')
                # 另一种就是 Image.open(StringIO(buffer)) 
                im = Image.open(StringIO(img['body']))
                # 修改图片大小resize接受两个参数, 第一个是宽高的元组数据,第二个是对图片细节的处理，本文表示抗锯齿
                im = im.resize((72, 72), Image.ANTIALIAS)
                # 打开io 就像文件一样
                im_file = StringIO()
                im.save(im_file, format='png')
                # 这是获取io中的内容
                im_data = im_file.getvalue() 
                f.write(im_data)
```
这样就可以上传的时候对文件大小进行修改

