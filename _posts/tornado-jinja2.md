title: tornado使用jinja2模板引擎
date: 2015-04-29 17:14:30
categories: tornado
tags: [tornado, python]
---
**tornado** 默认有一个模板引擎但是功能简单(其实我能用到的都差不多)使用起来颇为麻烦, 而`jinja2`语法与`django`模板相似所以决定使用他. 

### 下载jinja2
还是用`pip` 下载(用的真是爽)
``` bash
pip install jinja2
```
这样就可以使用了.

### tornado与jinja2 整合
tornado和jinja2整合起来很简单(其实是网上找的比较简单), 不知道从那里找到的反正找到了,不说了直接上代码
```python
#coding:utf-8

import tornado.web
from jinja2 import Environment, FileSystemLoader, TemplateNotFound

class TemplateRendring(object):
    """
    A simple class to hold methods for rendering templates.
    """
    def render_template(self, template_name, **kwargs):
        template_dirs = []
        if self.settings.get('template_path', ''):
            template_dirs.append(self.settings['template_path'])
        env = Environment(loader=FileSystemLoader(template_dirs))

        try:
            template = env.get_template(template_name)
        except TemplateNotFound:
            raise TemplateNotFound(template_name)
        content = template.render(kwargs)
        return content 

# 就是重新写 BaseHandler 由jinja2模板渲染
class BaseHandler(tornado.web.RequestHandler, TemplateRendering):
    """
    Tornado RequestHandler subclass.
    """
    def initialize(self):
        pass

    def get_current_user(self):
        user = self.get_secure_cookie('user')
        return user if user else None

    def render_html(self, template_name, **kwargs):
        kwargs.update({
            'settings': self.settings,
            'STATIC_URL': self.settings.get('static_url_prefix', '/static/'),
            'request': self.request,
            'current_user': self.current_user,
            'xsrf_token': self.xsrf_token,
            'xsrf_form_html': self.xsrf_form_html,
        })
        content = self.render_template(template_name, **kwargs)
        self.write(content)
```
这样就可以在tornado中替换`self.render`为`self.render_html`了.
