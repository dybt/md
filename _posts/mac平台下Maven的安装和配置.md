---
title: mac平台下Maven的安装和配置
date: 2016-05-20 22:36:47
tags:
---
### Maven 安装
下载[Maven](http://maven.apache.org/download.cgi)，并解压到一个文件夹如：/Users/CF/tools/apache-maven-3.3.9

### 创建软连接
``` bash
$ ln -s apache-maven-3.3.9 maven
```
maven是apache-maven-3.3.9的一个映射或者说是一个快捷方式。可以通过/Users/CF/tools/maven 访问文件夹
### 配置环境变量
在用户的根目录下编辑.bash_profile文件，.bash_profile是隐藏文件，可以通过 ls -la查看
{% img /img/20160420/2.png 1394 956  %}
vim编辑器添加以下几行记录，M2_HOME为maven的安装目录，JAVA_HOME为jdk的安装目录。具体目录根据实际情况调整
``` bash
# maven
export M2_HOME=/Users/CF/tools/maven
export M2=$M2_HOME/bin
export PATH=${PATH}:$M2
export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.7.0_75.jdk/Contents/Home
```
执行以下命令，是更改环境变量立即生效
``` bash
source .bash_profile
```
### 输入mvn -v 查看是否安装成功
{% img /img/20160420/3.png 1394 956  %}

### 安装ojdbc6-11.2.0.1.0.jav
``` bash
$ mvn install:install-file -DgroupId=com.oracle -DartifactId=ojdbc6 -Dversion=11.2.0.1.0 -Dpackaging=jar -Dfile=ojdbc6-11.2.0.1.0.jar 
```