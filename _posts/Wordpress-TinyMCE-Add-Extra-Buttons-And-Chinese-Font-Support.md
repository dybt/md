---
title:	WordPress默认编辑器TinyMCE增加额外按钮和中文字体支持
date: 2016-5-18 20:22:33  
tags:
  - Wordpress
  - TinyMCE
categories: Wordpress
---
## 前言

对于开发`Wordpress`来说，客户的要求可谓是五花八门，最近在做学校的新闻网改版的时候，就遇到一个小问题，需要在后台编辑文章的时候能够修改字体，字号以及字体颜色等功能，这些功能`Wordpress`后台默认是没有的，一开始我的想法是找一款第三方插件来实现这个功能，但是找来找去也没找到合适的插件，而且我平时开发的时候也是比较抵触插件的，能自己实现的绝不用插件去实现。四处寻觅无果，无意看了一下`Wordpress`官方的`API`函数。苍天有眼啊！原来，后台的编辑器可以插入很多增强功能。几乎包含所有的编辑器按钮。只是程序默认的功能比较少而已！

<!-- more -->

## 修改前后对比
修改前：
![](/images/2016/05/3.png)
修改后：
![](/images/2016/05/4.png)

## 为`TinyMCE`增加额外按钮

将下面代码添加到你的主题`functions.php`模版文件中：

```php
function enable_more_buttons($buttons) {
     $buttons[] = 'hr';
     $buttons[] = 'del';
     $buttons[] = 'sub';
     $buttons[] = 'sup';
     $buttons[] = 'fontselect';
     $buttons[] = 'fontsizeselect';
     $buttons[] = 'cleanup';
     $buttons[] = 'styleselect';
     $buttons[] = 'wp_page';
     $buttons[] = 'anchor';
     $buttons[] = 'backcolor';
     return $buttons;
     }
add_filter("mce_buttons_3", "enable_more_buttons");
```

## 为`TinyMCE`增加中文字体

```php
function custum_fontfamily($initArray){
   $initArray['font_formats'] = "微软雅黑='微软雅黑';宋体='宋体';黑体='黑体';仿宋='仿宋';楷体='楷体';隶书='隶书';幼圆='幼圆';";
   return $initArray;
}
add_filter('tiny_mce_before_init', 'custum_fontfamily');
```

## `wordpress`编辑器按钮调用的`Key`

1.剪切（cut）复制（copy）粘贴（paste）撤销（undo）重做（redo）居中（justifycenter）
2.加粗（bold）斜体（italic）左对齐（justifyleft）右对齐（justfyright）
3.两端对齐（justfyfull）插入超链接（link）取消超链接（unlink）插入图片（image）
4.清除格式（removeformat）下划线（underline）删除线（strikethrough）
5.锚文本（anchor）新建文本（newdocument）
6.字体颜色（forecolor）字体背景色（backcolor）
7.格式选择（formmatselect）字体选择（fontselect）字号选择（fontsizeselect）
8.样式选择（styleselect）无序列表（bullist）编号列表（numlist）
9.减少缩进（outdent）缩进（indent）帮助（wp_help）
10打开HTML代码编辑器（code）水平线（hr）清除冗余代码（cleanup）
11.上标（sub）下标（sup）特殊符号（charmap）插入more标签（wp_more）
12.插入分页标签（wp_page）
13.隐藏按钮显示开关（wp_adv）
14.隐藏按钮区起始部分（wp_adv_start）
15.隐藏按钮区结束部分（wp_adv_end）
16.拼写检查（spellchecker）

大功告成！