title: Vim练习
date: 2016-01-30 08:26:54
categories: 编程
tags:
- vim 
---
![图片来自互联网](http://7sbxbm.com1.z0.glb.clouddn.com/vimlogo.png)
*Vim编辑器学习练习笔记*
<!-- more -->
<!-- toc -->
### 1、VIM配置
vim配置默认放在用户目录下的`.vimrc`文件
```
"-----------------------------
" 配置
"----------------------------- 
" 显示行号
set nu
" 编码
set encoding=utf-8
" tab shift宽度
set ts=4
set sw=4
"语法高亮
syntax on
" 自动缩进
set autoindent
" 自动识别文件类型，plugin脚本，缩进定义
filetype plugin indent on
" 更该注释颜色
highlight Comment ctermfg=green guifg=green

"-----------------------------
" 快捷键配置
"----------------------------- 
" -*- 编辑模式 -*-
"输入() [] {} " 并回到()的中间
imap ( ()<Esc>i
imap [ []<Esc>i
imap { {}<Esc>i
imap " "<Esc>i
imap ( ()<Esc>i)""}])"
```
### 2、滚动屏幕
|动作|命令／快捷键|
|------|------|
|向后滚动一屏|Ctrl+f|
|向后滚动半屏|Ctrl+d|
|向前滚动一屏|Ctrl+b|
|向前滚动半屏|Ctrl+u|
|当前行置于屏幕顶端|zt|
|当前行置于屏幕底端|zb|


