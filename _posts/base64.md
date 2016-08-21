title: python base64 编码
date: 2015-04-15 16:05:02
categories: python
tags: [python, 标准库]
---
### base64 编码
base64用来编码二进制数据,其原理可查看[廖雪峰的官方网站][1].
[1]: http://www.liaoxuefeng.com "廖雪峰的官方网站"
首先准备一个64个字符的数组['A','B','C', ... , '1', '2', ... , '+']
然后对二进制数据进行处理, 每三个字节(byte)一组, 一共是3x8=24, 划为4组,每组6bit.这样我们得到4个数字作为索引, 然后查表获得相应的4个字符, 就是编码后的字符串.
如果编码的二进制数不是3的倍数怎么办,base64在`\x00`在末尾不足后,在代码的末尾添加1~2个`=`号,解码是自动去掉
<!--more-->
```python    
    import base64

    me = base64.b64encode("mink")
    print me                        # print 'bWluaw=='
```
会把少的字符用=号补上
```python
    print base64.b64decode(me)      # print 'mink'
```
可以解码得到原有的字符串
### urlsafe_b64encode 和 urlsafe_b64decode
base64的还提供用于url的编码方法*urlsafe_b64encode* 和*urlsafe_b64decode*.
```python
    me = base64.urlsafe_b64encode('i\xb7\x1d\xfb\xef\xff')
    print me                        # print 'abcd--__'

    print base64.urlsafe_b64decode(me)  print 'abcd++//'
```
因为base64编码后**+**和**-**可能会出现在字符中,所以通过urlsafe的方法把**+**转换为**-**,**/**转化为**\_**.
>由于=字符也可能出现在Base64编码中,但=用在URL Cookie里面会造成歧义,很多Base64编码后会把=去掉.去掉=后怎么解码呢？因为Base64是把3个字节变为4个字节,所以Base64编码的长度永远是4的倍数.因此需要加上=把Base64字符串的长度变为4的倍数,就可以正常解码了.


    

    
