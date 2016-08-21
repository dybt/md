title: 使用Hexo+Github+Ubuntu搭建博客 
date: 2016-02-26 22:38:37
tags: Others
---
### Ubuntu
+ 安装nodejs
```bash
sudo apt-get install nodejs
```
+ 安装git
```bash
sudo apt-get install git
```

<!-- more -->

### Github
+ 需要有github帐号，然后在github上新建一个仓库，命名为username.github.io
+ 进行git ssh 认证，[参考](https://segmentfault.com/a/1190000002645623)

### Hexo使用
+ 安装nodejs,然后使用npm安装hexo
```bash
npm install -g hexo
```
+ 创建博客文件夹
```bash
hexo init 文件夹的名字(假设为BLOG)
cd BLOG
npm install  //安装依赖
```
+ git配置
```bash
cd BLOG
git config --global user.name "username" // username为你自己的github用户名
git config --global user.email "email@example.com"
```
  - 为了用于保存hexo的代码,防止hexo代码遗失，并方便不同平台博客编辑的同步,可以将hexo的代码保存到github上，方式是新建一个仓库，并进行并联
```bash
git remote add origin git://github.com...  //关联远程仓库, git.github.com..要根据你在github上建立出来的仓库为准
```
+ 修改配置文件
    * 修改BLOG目录下的_config.yml里面的Deployment部分为
```
deploy:
  type: git
  repo: git@github.com:username/username.github.io.git
  branch: master
```
+ 常用命令
    * hexo g //generate ,编译成静态文件
    * hexo d //deploy, 部署网站
    * hexo s //server, 本地运行
    * hexo c //clean, 清空generate生成器的文件
+ 新建博文
    * 先建立文件
```bash
hexo new post "newPost"
```
    * 编辑/source/_post/newPost.md,使用的是markdowm语法，其语法可以参考[链接](http://hp256.com/2014/12/23/post-1/)
    * 关于编辑的工具可以在sublime里面安一些关于markdown的插件，也可以使用markdowm cmd等工具来编辑
    * 编辑完成后
```bash
hexo generate
hexo deploy
hexo server //本地查看
```
+ 关于主题
    * 本人使用的是 [yilia](https://github.com/litten/hexo-theme-yilia)

+ 访问自己的博客:https://username.github.io/  (username为自己的用户名)

### Hexo经验
+ 使用`hexo g`有时会出现
```
TypeError: Cannot read property 'compile' of undefined
```
解决的方法降低使用的hexo的版本,打开package.json
```
  "dependencies": {
    "hexo": "^3.1.0",   //这里修改为 3.1.1
    "hexo-deployer-git": "^0.1.0",
    "hexo-generator-archive": "^0.1.2",
    "hexo-generator-category": "^0.1.2",
    "hexo-generator-index": "^0.1.2",
    "hexo-generator-tag": "^0.1.1",
    "hexo-renderer-ejs": "^0.1.0",
    "hexo-renderer-jade": "^0.2.0",
    "hexo-renderer-less": "^0.1.3",
    "hexo-renderer-marked": "^0.2.4",
    "hexo-renderer-stylus": "^0.3.0",
    "hexo-server": "^0.1.2"
  }
```
修改好hexo的版本后再`npm install`更新下版本就可以了