---
title: fabric 使用简介
date: 2016-05-24 16:34:09
tags: [python, fabric, 远程部署]
---
fabric是python[2.5-2.7]写的命令行部署程序的库。

```python
# 使用fabric 必须把文件名改成fabfile.py (可以通过-f 来指定文件名)
from fabric.api import run, local

def name():
    local('uname -s')
```
`run`用于远程服务器, `local`是本地。

```python
from fabric.api import run, local, env, cd, lcd, put

base_dir = os.path.dirname(os.path.abspath(__file__))
pro_file = '{}.tar.gz'.format(base_dir.split('/')[-1])

# env 来配置配置环境
env.hosts = ['username@address:port', 'username@address:port']
env.password = 'xxx'

def local_setting():
    # 通过上下文 lcd 切换本地目录
    with lcd('/home/mink/'):
        # 在本地文件进行压缩, put用于上传文件
        local('tar zcvf {} {}'.format(pro_file, os.path.basename(base_dir)))
        put(pro_file, '/home/ubuntu/{}'.format(pro_file))

def server_setting():
    # 上下文 cd 切换远程服务器目录
    with cd('~/'):
        # 远程服务器上执行命令
        run('tar zxvf {}'.format(pro_file))

def update():
    local_setting()
    server_setting()
```
这样就完成了简单的部署。
