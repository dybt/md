title: ubuntu软件与使用
date: 2016-02-28 11:25:47
tags:
---

#### 截图工具
+ 用内置的相关设置如下
```bash
gnome-screenshot -h (help)
gnome-screenshot -w (window)(抓取窗口，不是整个屏幕)
gnome-screenshot -a (抓取区域）
gnome-screenshot -B 去除屏幕截图的窗口边框 
```
+ 实现按下Ctrl+Alt+A 来实现区域截图
    - 依次打开 系统设置 > 键盘 > 快捷键 > 自定义快捷键 > +，在【name】输入 screenshot , 【command】输入 gnome-screenshot -a ，点击【apply】确定后，再点击disable 禁用，接着就同时按下 Ctrl+Alt +A 就可以成功设置截图快捷键了

<!-- more -->

#### 系统变量的修改
+ 我下载了一个Fiji的安装包，其中有运行文件，我希望可以直接在命令行下输入可执行文件就可以运行，而不是到可执行文件的目录下才可以，此时需要做的是修改/etc/profile文件（需要root权限），修改如下在最后加入
```bash
export FIJI=/home/qinkai/.Fiji.app
export PATH=$FIJI:$PATH
```
+ 其中.Fiji.app是可执行文件fiji所在的目录。修改完后，重新启动系统，系统就可以加载新的/etc/profile了。如果想直接试验也可以在一个终端中输入“source /etc/profile”这样这个终端就加载了新的/etc/profile了

#### 将其他格式的文件转为UTF-8格式
```bash
# 安装
sudo apt-get install enca
# 转换 命令格式如下
# enca -L 当前语言 -x 目标编码 文件名
# 例如要把当前目录下的所有文件都转成utf-8
enca -L zh_CN -x utf-8 *
```

#### 打开可视目录
+ nautilus .

#### 安装可以在命令行使用的词典
+ [参考](http://www.mintos.org/apps/stardict.html)
+ 安装
```
# 安装
sudo apt-get install sdcv
sudo apt-get install stardict
# 字典下载 http://download.huzheng.org/
```
+ 命令 `sdcv`

#### 开个wifi
+ 参考 [百度](tudou.com/programs/view/skHcb8dqvB8)
+ 注意最后一步`最后,点击 保存 保存这个网络吧。连接刚才建立的网络,打开 连接到隐藏的网络。选择之前建立的网络连接即可。注意这个无线是把现在电脑上的网络共享出去,所以在此之前请连接好你的宽带什么的。`