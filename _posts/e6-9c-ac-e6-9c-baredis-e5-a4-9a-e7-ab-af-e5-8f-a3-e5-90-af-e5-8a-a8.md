---
title: 本机Redis多端口启动
id: 239
categories:
  - redis
date: 2016-04-23 06:09:04
tags:
    - redis
---

最近再看redis,看到redis的主从复制模块,但因吃土的日子没有多台服务器,于是尝试着在本地启动多端口,简单实现复制.

根据网上的教程找了半天没有找到redis的安装路径,原来是cenotsyum安装的,最后只有一个redis-server文件,通过find搜索发现,,只是在在我的/etc目录下有一个redis.conf,后来搜索资料发现,redis的正常启动流程默认是要读取配置文件的,

应该是用redis-server redis.conf这样即可, 如果想要设置多端口,需要复制一份redis.conf 然后启动即可.