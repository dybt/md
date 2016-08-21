title: 前端CSS种种问题收集
date: 2016-07-31 15:20:13
tags:
- Web
- CSS
---
> 每个问题都会加上对应的资料

### overflow的种种
+ 问题
  - 编写css时我们经常遇到，如果父元素div里面只包含浮动元素的话，子元素会溢出父元素，这时我们的做法是对父元素使用overflow:hidden；那是这是为何呢？
+ 解决 
  - 关键在于使用overflow:hidden会生成一个BFC，更多请 [参考](http://harttle.com/2016/05/11/block-formatting-context.html)
<!--more-->

### jquery里面的prop与attr
+ 参考 [详细](http://www.cnblogs.com/dolphinX/p/3348582.html)
+ 自己的坑
  - 情况：使用的jquery版本为>1.6,书写了如下的代码
  ```
    function toggleChecked(target) {
      console.log($(target).attr('checked'))
        //错误代码
        /* if (!$(target).attr('checked')) {
            $(target).attr('checked', 'checked');
        } else {
            $(target).removeAttr('checked');
        }*/
        //正确代码
        if(!$(target).prop('checked')) {
          $(target).prop('checked', true);
        } else {
          $(target.prop('checked', false))
        }
    }
  ```
    - 如上的错误代码在表现的情况是尽管checked属性修改了，但是元素没有打勾。 [参考](http://blog.sina.com.cn/s/blog_6657f20e0101g793.html)

### css里面的外边距叠加
+ 情况
  - 代码如下
  ```
  /*html*/
  <div class="product_header">
    <h1>产品信息</h1>
    <div>产品名称: {{product.name}}</div>
    <div>产品描述: {{product.detail}}</div>
  </div>

  /*css*/
  .product_header {
    padding: 1px;
    background-color: gray;
  }
  .product_header h1{
    margin-top: 100px;
  }
  ```
  - 上面代码的原本意图是希望h1的距离外部的父元素的顶部有100px的，可是出现的效果却是整个父元素向下移动了，这个就是外边距叠加的问题
+ 概念
  - 具有块级子元素的元素在计算其高度的时候，如果其本身没有垂直边框(border-left, border-right)或内边距(padding)的话，那么它的高度就是它包含的子元素的顶部和底部边框之间的距离，因此会导致包含的子元素的顶部和底部外边距突出到容器元素的外边；体现的效果就是整体的位置因为外边距的关系整体下移了
+ 解决的方法
  - 给父元素添加一个垂直边框或内边距，比如`padding:1px`,这样外边距就不会叠加了