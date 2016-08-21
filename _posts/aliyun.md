title: Flask在阿里云搭建服务
date: 2015-11-03 16:05:40
categories: python
tags: [阿里云, flask, gunicorn, supervisor]
---
### 项目运行环境
阿里云(单核CPU, 1G内存, Ubuntu 14.04 x64 带宽1Mbps), 具体购买和ssh连接阿里云本文不做描述。
#### 实用工具
首先进入阿里云后先要升级下`apt-get`, 并下载所需软件
```bash
sudo apt-get update
sudo apt-get install vim git wget tmux
```
我还会使用zsh和oh-my-zsh来替换bash
```bash
sudo apt-get install zsh

# 终端下打以下命令
wget --no-check-certificate https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh
```
替换bash为zsh
```bash
chsh -s /bin/zsh
```
重新连接阿里云就可以看到效果, 具体主题可以根据自己喜好更改主目录下的`.zshrc`即可

#### 安装python相关模块
使用python的pip包管理工具
```bash
sudo apt-get install python-setuptools
sudo apt-get install python-pip
sudo pip install python-virtualenv
```
<!--more-->
在主目录下创建文件夹`code`并把所有项目放进这个目录里
```bash
mkdir code

# 使用virtualenv
vritualenv venv

# 开启virtualenv
source /venv/bin/activate

# 关闭virtualenv
deactivate

# 在开启virtualenv的状态下使用pip会使用venv中的pip，也就是所有模块都会安装到venv中
# 如果想要安装全局模块就使用sudo pip 

# 安装flask, 安装到venv中
pip install flask

# 两个是不同的, 安装到系统中
sudo pip install flask
```

### 使用Flask
现在我们已经安装了Flask, 现在我们写一个小的程序来运行一下。
这里我们在一次安装`flask`的相关模块,并把他们写入到`requirements.txt`这个文件中。
```bash
# 安装flask的相关模块
pip install -r requirements

# 创建一个manage.py文件, 用作管理我们的应用, 然后创建我们的一个应用。
touch manage.py

## 创建一个叫bamboo的应用
mkdir bamboo

## 创建一个应用的文件, 接下来我们要编辑这个文件了。
touch bamboo/__init__.py bamboo/app.py
```
项目目录结构是这样的
```bash
/root/code/
├── bamboo
│   ├── app.py
│   ├── config.py
│   ├── __init__.py
│   ├── static
│   ├── templates
│   └── views
├── gunicorn.conf
├── logs
│   ├── access.log
│   ├── error.log
│   ├── supervisord.log
│   ├── supervisor_err.log
│   └── supervisor_out.log
├── manage.py
├── requirements.txt
├── run.py
├── sweep.py
└── venv
```
编辑bamboo/app.py和bamboo/views/main.py
```python
## app.py
from flask import Flask

def create_app():
    app = Flask(__name__)

    # do something

    register_blueprints(app)
    return app


# 用来注册视图的路由
def register_blueprints(app):
    from bamboo.views import main, error
    for i in (main, error):
        app.register_blueprint(i.bp)

## main.py
from flask import render_template, Blueprint

bp = Blueprint('main', __name__)


@bp.route('/')
@bp.route('/index')
def index():
    return '<h1>Hello, Bamboo!</h1>'
```
### 安装Gunicorn
Gunicorn是一个wsgi服务器, 我们将通过它来启动我们的web服务。
```bash
# 注意我们是通过pip来安装，所以该模块所有文件都是在venv里面
pip install gunicorn
```
可以进入目录vevn/bin下查看是否存在gunicorn命令
```python
# 这里我们编辑下manage.py文件，具体文件路径请查看上文中的目录结构
## manage.py

from bamboo.app import create_app
from flask.ext.script import Manager, Server, Shell

app = create_app()
manager = Manager(app)

def make_shell_context():
    #　用于shell使用
    pass

manager.add_command('shell', Shell(make_context=make_shell_context))
manager.add_command('runserver', Server(
    use_debugger=True,
    use_reloader=True,
    host='127.0.0.1',
    port=5000
))

if __name__ == '__main__':
    manager.run()
```
运行一下manage.py
```bash
# 不适用gunicorn运行
python manage.py runserver

# 使用gunicorn 运行
# 这里需要注意一下, 冒号前面的是文件名也就是manage.py而后面的是应用的名称。我在这里载过跟头。因为服务器需要接受一个wsgi的应用而manager = Manager(app)不是一个wsgi应用所以报错
gunicorn manage:app
```
我们创建一个gunicorn配置文件gunicorn.conf并把他放到code目录下
```bash
## gunicorn.conf

workers=3
bind='127.0.0.1:5000'
```
### 安装Nginx
我们通过nginx来反向代理我们的服务。
```bash
# 首先下载一个nginx
sudo apt-get install nginx
```
我们可以访问我们的域名或IP地址，如果出现Nginx的提示就表示安装成功了。
nginx安装在/etc/nginx/目录下,　我们把项目的配置文件放到/etc/nginx/sites-available/
```bash
server{
    listen 80;
    server_name xxx.xxx.xxx.xxx;

    # nginx log信息, 需要创建logs目录
    access_log /root/code/logs/access.log
    error_log /root/code/logs/error.log

    location / {
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header HOST $host;
        proxy_redirect off;
        if (!-f $request_filename){
            proxy_pass http://127.0.0.1:5000;
            break;
        }
    }
}
```
需要创建一个软链接到/etc/nginx/sites-enabled/
```bash
# 这里bamoo只是文件名你也可以取自己的项目名称
ln -s /etc/nginx/sites-available/bamboo /etc/nginx/sites-enabled/bamboo

# 然后检查nginx有没有错误
nginx -t 

# 重新启动服务
service nginx restart
```

### 安装Supervisor
supervisor用来监控进程，并在进程挂掉的时候自动重启它。
```bash
# 这里需要把它安装到系统中
sudo pip install supervisor

# 生成配置文件
sudo echo_supervisord_conf > /etc/supervisord.conf
# 如果说权限不足, 在ubuntu下可以使用, -i 来提取权限
sudo -i
echo_supervisord_conf > /etc/supervisord.conf

# 编辑/etc/supervisord.conf,并在最后一行加入一下字段
# 这样配置文件会将/etc/supervisor/conf.d下所有.conf结尾的都会导入进来
[include]
files = /etc/supervisor/conf.d/*.conf

# 在创建一个配置文件到/etc/supervisor/conf.d/bamboo.conf
[program:bamboo]
command=/root/code/venv/bin/gunicorn manage:app -c /root/code/gunicorn.conf
directory=/root/code/
user=root
autostart=true
autorestart=true
stdout_logfile=/root/code/logs/supervisor_out.log
stderr_logfile=/root/code/logs/supervisor_err.log
```
通过supervisord命令启动supervisor进程
```bash
# 通过supervisord 加载配置文件并启动supervisor
supervisord -c /etc/supervisord.conf
```
通过supervisorctl工具用来管理supervisor维护的进程
```bash
# reread来检测修改的配置内容, update来更新
# 这样就可以看到bamboo的process启动了
supervisorctl reread
supervisorctl update 

# 也可以通过supervisorctl status 查看所有的项目进程
bamboo                           RUNNING   pid 12928, uptime 2:13:05
```
这样我们的web服务已经启动起来了。

有时候我们需要开机自启动程序，这需要把supervisor启动成一个服务。在ubuntu系统下通过安装脚本来完成
```bash
# 下载脚本
sudo su - root -c "sudo curl https://gist.githubusercontent.com/howthebodyworks/176149/raw/d60b505a585dda836fadecca8f6b03884153196b/supervisord.sh > /etc/init.d/supervisord"
# 添加执行权限
sudo chmod +x /etc/init.d/supervisord
# 设置开机自动运行
sudo update-rc.d supervisord defaults
```
下载的脚本可能不能运行，需要对路径进行修改比如
```bash
DAEMON=/usr/bin/supervisord
# 改成 本机中的真实路径
DAEMON=/usr/local/bin/supervisord

DAEMON_ARGS=""
# 添加 arg
DAEMON_ARGS="-c /etc/supervisord.conf"

PIDFILE='/var/run/$NAME.pid'
PIDFILE='/tmp/$NAME.pid'
```
然后重启后就可以看到，supervisor服务已经开启了。
### 参考
1. [VPS环境搭建详解](http://beiyuu.com/vps-config-python-vitrualenv-flask-gunicorn-supervisor-nginx/)
2. [How to run Flask app using gunicorn?](http://stackoverflow.com/questions/29265044/how-to-run-flask-app-using-gunicorn)
