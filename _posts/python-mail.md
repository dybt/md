title: Python发送邮件简单事例
date: 2015-12-03 14:24:54
categories: python
tags: [python, mail]
---

```python
import smtplib
from email.mime.text import MIMEText

to_list = ['123@123.com', '456@456.com']
server_host = 'smtp.163.com'

username = '你的邮箱账号'
password = '你的邮箱密码'


def send(to_list, sub, content):
    '''
    :param to_list: 收件人邮箱
    :param sub: 邮件标题
    :param content: 内容
    '''
    me = "manager" + "<" + username + ">" 
    # _subtype 可以设为html,默认是plain
    msg = MIMEText(content, _subtype='html')
    msg['Subject'] = sub
    msg['From'] = me
    msg['To'] = ';'.join(to_list)
    try:
        server = smtplib.SMTP()
        server.connect(server_host)
        server.login(username, password)
        server.sendmail(me, to_list, msg.as_string())
        server.close()
    except Exception as e:
        print str(e)

if __name__ == '__main__':
    send(to_list, "这个是一个邮件", "<h1>Hello, It's test email.</h1>")
```
