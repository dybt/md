title: 前端学习开发环境
date: 2016-02-26 21:29:33
tags: Web
---
### ubuntu环境的配置
+ mongodb的安装： sudo apt-get install mongodb
+ node的安装：推荐使用nvm的管理工具，
    - 安装命令如下：
```
git clone https://github.com/creationix/nvm.git ~/.nvm
cd ~/.nvm
git checkout `git describe --abbrev=0 --tags`
./install.sh

```
    - 参考 [网址3](http://www.clarkhan.com/2015/06/05/linux-install-node-with-nvm/)
+ 另外使用nvm时有时会遇到没有没有权限的问题，修改在.bashrc文件中加入`export NVM_DIR = ~/.nvm`或是`export NVM_DIR="/home/kinthon/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"  # This loads nvm`export NODE_PATH="/home/kinthon/.nvm/versions/node/v5.5.0/lib/node_modules"`
+ 另外需配置NODE_PATH，不然的话，全局安装的东西在文件里面没有办法使用``

+ [cnpm 安装](https://npm.taobao.org/)

+ sublime的安装与配置
    * 直接在[sublime官网](https://www.sublimetext.com/3)下载安装包，然后双击即可安装，安装完成后subl就可以启动软件了
    * 插件
        - 安装Package Control，[参考链接](http://my.oschina.net/chinesedragon/blog/160197)
        - 本人目前已安装的插件有
            + Emmet，用于快速编写html
            + HTML-CSS-JS Pretty，用于规范化代码分格的
            + Babel, 用于转换es6
            + CoolFormat： 代码格式化工具
            + markdownlighting
    * 关于HTML-CSS-JS Pretty插件，其是依靠node来运行的，所以需要指定node的路径，在windows下面的路径形式是
    ` "windows": "D:\\program-software\\nodejs\\node.exe`
    * emmet使用 [参考](http://bubkoo.com/2014/01/04/emmet-a-toolkit-for-improving-html-css-workflow/)
    * 关于sublime的一些个配置
```
{
    "hot_exit": false,
    "ignored_packages":
    [
    ],
    "remember_open_files": false,
    "tab_size": 2,
    "word_wrap": true
}
```
    * 关于build系统
        - 对于编程人员来说，这是个利器，因为通过这个就可以直接在sublime里面运行代码了，比如我想在这里运行es6的话,本人已经使用npm安装了babel了，直接在Tool->Build System->New Build System然后编辑文件如下，然后保存即可

```javascript
{
    "working_dir": "${project_path:${folder}}",
    "selector": "source.js",
    "encoding": "utf-8",
    "shell": true,
    "windows": {
        "cmd": ["taskkill /f /im node.exe >nul 2>nul & babel-node $file"]
    },
    "osx": {
        "cmd": ["killall node >/dev/null 2>&1; babel-node $file"]
    },
    "linux": {
        "cmd": ["killall node >/dev/null 2>&1; babel-node $file"]
    }
}
```
+ Chrome
    * ubuntu下安装Chrome也是下载安装包(.deb)就可以了
    * Chrome devtool工具的使用参考[链接1](http://www.cnblogs.com/wukenaihe/archive/2013/01/27/javascript%E8%B0%83%E8%AF%95.html),[链接2](https://developer.chrome.codevtools?hl=dm/)
    * 插件
        - ColorZila,用于取色
        - Page Ruler,用于衡量页面
        - EditThisCookie，用于编辑cookie
        - Page load time
        - Octotree, 树形展示Github 项目代码
    * 建议弄一个google帐号，因为用帐号登录了Chrome后，它可以帮你记住你的一些标签和使用的插件，这样你重新安装Chrome后，登录它就可以帮你同步标签和插件了
