title: Ubuntu下git的使用
date: 2016-02-27 12:39:07
tags: Others
---
#### 安装
```bash
sudo apt-get install git
```
#### 基本使用(以使用BLOG仓库为例)
+ 在github上新建一个仓库 BLOG
+ 本地git操作
```bash
# 初始化配置
git init BLOG
git config --global user.name "uesrname"
git config --global user.email "email"
# ssh认证 参考: https://segmentfault.com/a/1190000002645623
# 远程关联
git remote add origin git@github.com:username/BLOG.git
# 假设本地已有文件需要上传,相关的命令有
git add -A  # 上传全部文件
git commit -m "提交的描述"
git push -u origin master 
git push origin :huangqinkai //delete the remote branch 
```
### 其他命令
+ git add -f 
+ git remote rm origin
+ git commit -am #其中的-a选项只将所有被修改或者已删除的且已经被git管理的文档提交倒仓库中。如只是修改或者删除了已被Git 管理的文档，是没必要使用git add 命令的
- git pull origin master
- git clone
### .gitignore样本
``` bash
db.json
*.log
node_modules/
public/
.deploy*/
```
### 使用git gh-pages进行页面展示
+ 新建分支 取名为gh-pages,`git checkout -b gh-pages`
+ 在gh-pages分支上推上index.html文件，一定要有index.html文件，然后再通过这个文件里面的链接去链接其他的git同分支文件即可
+ 到github里面的setting下面有一个url，那个就可以访问到相应的内容了。
    