title: python socket 简单聊天室
date: 2015-06-11 14:14:57
categories: python
tags: [python, 标准库, ]
---
#### 关于socket
类型|注释
---|---
socket.AF_INET            |IPV4协议网络通讯
socket.AF_INET6           |IPV6协议网络通讯
socket.SOCK_STREAM        |tcp协议
socket.SOCK_DGRAM         |UDP协议

> 函数, sock 表示 socket对象.

方法|注释
---|---
sock.bind((HOST, PORT))   |绑定地址
sock.listen(num)          |最大连接数
sock.accpet()             |接受TCP连接返回(就是来自客户端的socket)，一个双元素元组数据
sock.connet((HOST, PORT)) |客户端用于连接服务器
sock.send(msg)            |socket 发送数据
sock.sendall(msg)         |socket 发送完整的tcp数据.
sock.recv(buffer)         |接受数据buffer是接受数据快大小, recv阻塞socket    
sock.sendto(msg)          |udp发送数据
sock.recvfrom(buffer)     |udp接受数据
sock.getpeername()        |来自socket的地址元组数据
sock.setsockopt()         |一些选项值
sock.setblocking(flag)    |0表示非阻塞, 默认阻塞
sock.close()              |关闭socket连接

<!--more-->
#### 简单socket半双工
用python的`socket`模块实现的简单半双工聊天服务器
```python
    # server.py
    import socket

    HOST = ''
    PORT = 5000

    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    sock.bind((HOST, PORT))
    sock.listen(5)

    while True:
        print '等待连接...'
        client, address = sock.accept()
        print '连接自...', address
        while True:
            data = client.recv(1024)
            if not data: break
            print address, '> ', data
            msg = raw_input('> ')
            if not msg: break
            client.sendall(msg)
    sock.close()
```
服务器端等待客户端连接并等待输入数据，如果客户端输出数据就打印出来并等待服务器输入数据，并发送给客户端.
```python
    # client.py

    import socket

    HOST = ''
    PORT = 5000

    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    sock.connect((HOST, PORT))

    while True:
        msg = raw_input('> ')
        if not msg: break
        sock.sendall(msg)
        data = sock.recv(1024)
        if not data: break
        print '服务器说: ', data

    sock.close()
```
这样半双工完成了，比较单调只能一来一回的发消息。以下是图片
<img src="http://miting-image.stor.sinaapp.com/QQ截图20150611144025.png" class="img-shadow" alt=""/>

#### 简单socket服务器
<hr>
在网上找了点资料修修改改终于可以运行了(大部分例子都是一样的).
```python
    # server.py

    import socket
    import select
    import Queue
    import threading

    class Server(threading.Thread):
        def __init__(self):
            super(Server, self).__init__()
            self.sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
            # 端口复用
            self.sock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
            # 非阻塞
            self.sock.setblocking(0)
            self.sock.bind(('', 5000))
            # 最大连接数,超过部分会等待
            self.sock.listen(5)
            # socket 可读状态列表
            self.read_list = [self.sock]
            # socket 可写状态列表
            self.write_list = []
            # socket 的消息队列
            self.msg_que = {}
            # socket server 超时设置
            self.time = 30

        def run(self):
            print '等待连接...'
            while True:
                # 监听 socket的状态
                read_sock, write_sock, error_sock = select.select(
                                                            self.read_list,
                                                            self.write_list,
                                                            [],
                                                            self.time)
                if not (read_sock or write_sock or error_sock):
                    print '连接超时..'
                    break

                # 迭代可读状态并判断是否是客户端
                for s in read_sock:
                    if s is self.sock:
                        # 如果是服务器就接受来自客户端的连接,并天价到可读状态列表
                        client, address = s.accept()
                        print '连接自...', address
                        self.read_list.append(client)
                        self.msg_que[client] = Queue.Queue()
                    else:
                        # 如果是客户端socket就接受来自客户端的数据
                        data = s.recv(1024)
                        if data:
                            # 判断是否退出
                            if data.startswith('[q]')
                                print str(s.getpeername()) + '  ' + data.split('[q]')[1]
                                self.read_list.remove(s)
                                s.close()
                            else:
                                # 不是退出状态就把数据print 到终端
                                print s.getpeername(), 'say: ' + data
                                self.msg_que[s].put(data)
                                if s not in self.write_list:
                                    self.write_list.append(s)
                        else:
                            if s in self.write_list:
                                self.write_list.remove(s)
                            self.read_list.remove(s)
                            s.close()
                            del self.msg_que[s]

                    for s in write_sock:
                        try:
                            msg = self.msg_que[s].get_nowait()
                        except Queue.Empty:
                            self.write_list.remove(s)
                        else
                            s.send(msg)

    if __name__ == '__main__':
        server = Server()
        server.start()
```
服务器部分来接受客户端的连接，并从客户端内容返回到终端上。

```python
# client.py

import socket
import threading

class Client(threading.Thread):
    def __init__(self):
        super(Client, self).__init__()
        self.sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        self.sock.connect(('', 5000))

    def run(self):
        while True:
            msg = raw_input('> ')
            if not msg:
                msg = '[q]退出聊天室.'
                self.sock.send(msg)
                break
            self.sock.send(msg)
            try:
                data = self.sock.recv(1024)
                if not data: break
            except:
                pass
if __name__ == '__main__':
    client = Client()
    client.start()
```

客户端部分变化的不多就是用了线程。
 
#### 参考文献
* <a href="http://yangrong.blog.51cto.com/6945369/1339593">python socket编程详细介绍</a>
* <a href="http://www.cnblogs.com/hazir/p/python_chat_room.html">Python Socket 编程——聊天室示例程序</a>
* <a href="http://blog.csdn.net/orangleliu/article/details/8840929">[Python]Socket高级 -- select I/O复用模型（二）
</a>
