title: 配置vim插件
date: 2015-06-05 11:57:40
tags: [vim,]
---
以前一直把`vim`当编辑器用,虽然有些地方很方便但还是感觉没有IDE强大.这不符合`神器`的说法.在最初学习`python`的时网上大部分(Linux环境下)让选择`vim`, 所以我给他安装些插件来增强下功能.
### vim 管理插件 Vundle
<hr>
我使用系统是 ubuntu 14.04 默认已经安装了`vim`和`git`. 如果机器中没有`vim`和`git`.
```bash
$ sudo apt-get install vim
$ sudo apt-get install git 
```
`vundle` 是vim的一个插件管理的工具类似于python中的`pip`, 从github下载<a href="https://github.com/gmarik/Vundle.vim">Vundle</a>
```bash
mkdir ~/.vim
mkdir ~/.vim/bundle
```
<!--more-->
然后使用git下载
```bash
git clone https://github.com/gmarik/Vundle.vim.git ~/.vim/bundle/vundle
```
然后配置`.vimrc`内容, 如果以前没有配置过`.vimrc`的话这个文件是不存在的
```bash
vim ~/.vimrc
```
然后在`.vimrc`下添加以下内容
```bash
set nocompatible
filetype off
set rtp+=~/.vim/bundle/vundle
call vundle#begin()

call vundle#end()
filetype  plugin indent on
```
在 `call vaundle#bengin()` 和`end()`中设置插件信息,如下:
```bash

Plugin 'scrooloose/nerdtree'
Plugin 'majutsushi/tagbar'
Plugin 'bling/vim-airline'
Plugin 'Pydiction'
Plugin 'mattn/emmet-vim'

```
然后对这些插件进行配置,按个人喜好
我是`F2`来开启和关闭`nerdtree`, `F5`开启和关闭`tagbar`.`ctrl + e`来使用`emmet`(以前叫zen coding).

```bash
map <F2> :NERDTreeToggle<CR> 
autocmd bufenter * if (winnr("$") == 1 && exists("b:NERDTreeType") && b:NERDTreeType == "primary") | q | endif

nmap <F5> :TagbarToogle<CR>
let g:tagbar_width=30

let g:airline_powerline_fonts = 1
let g:airline#extensions#tabline#enabled = 1
let g:airline#extensions#tabline#left_sep = ' '
let g:airline#extensions#tabline#left_alt_sep = '|'

let g:pydiction_location = '~/.vim/bundle/Pydiction/complete-dict'

let g:user_emmet_expandabbr_key='<c-e>'
let g:user_emmet_settings={'indentation':' '}

```
保存这些信息后使用`vim`打开进入vim任意文档,执行安装命令
```bash
vim 
:PluginInstall
```
这时你就可以看到插件在逐个安装,当看到`Done`是表示可以结束了.





