title: python asyncore 模块
date: 2015-08-17 14:21:47
categories: python
tags: [python, 标准库]
---
这个模块是socket的异步实现。

### asyncore.loop
输入一个轮询循环直到通过计数或打开的通道已关闭。

### asyncore.dispatcher
dispatcher类是一个底层socket类的包装对象。要使它更有用, 它有一部分事件处理方法被异步循环调用。否则它就是一个标准的非阻塞socket对象。
底层的事件在特定事件或特定的连接状态告诉异步循环,某些高级事件发生了。例如,　我们要求一个socket连接到另一个主机。
<!--more-->

* handle_connect() 第一次读或写事件。
* handle_close() 读事件没有数据可用。
* handle_accept  读事件监听一个socket。

#### handle_read
在异步循环察觉到通道呼叫read()时调用。

#### handle_write
在异步循环检测到一个可写的socket可以写的时候调用。这种方法经常实现缓冲性能。比如

```python
def handle_write(self):
    sent = self.send(self.buffer)
    self.buffer = self.buffer[sent:]
```

#### handle_expt
当有(OOB)数据套接字连接。这几乎永远不会发生,因为OOB精细地支持和很少使用。

#### handle_connect
当socket创建一个连接时调用。

#### handle_close
当socket连接关闭时调用。

#### handle_error
当引发一个异常并没有其他处理时调用。

#### handle_accept
当本地监听通道与远程端建立连接(被动连接)时调用。

#### readable
每次在异步循环确定是否添加一个通道socket到读事件列表时调用,默认都为True。

#### writable
每次在异步循环确定是否添加一个通道socket到写事件列表时调用, 默认为True。

#### create_socket
与创建标准socket的时候相同。

#### connect
与标准socket的端口设置是相同, 接受一个元组第一个参数为主机地址,第二个参数是端口号。

#### send
向远程端socket发送数据。

#### recv
从远程端socket读取最多buffer_size的数据。一个空的字符串意味着从另一端通道已关闭。

#### listen
监听socket连接。

#### bind
将socket绑定到地址。

#### accept
接受一个连接,　必须绑定到一个socket和监听地址。

#### close
关闭socket。

### asyncore.dispatcher_with_send
dispatcher子类添加了简单的缓冲输出功能用于简单的客户,更复杂的使用asynchat.async_chat。

### asyncore.file_dispatcher
file_dispatcher需要一个文件描述符或文件对象地图以及一个可选的参数,包装,使用调查()或循环()函数。如果提供一个文件对象或任何fileno()方法,该方法将调用和传递到file_wrapper构造函数。可用性:UNIX。

### asyncore.file_wrapper
file_wrapper需要一个整数文件描述符并调用os.dup()复制处理,这样原来的处理可能是独立于file_wrapper关闭。这个类实现足够的方法来模拟一个套接字使用file_dispatcher类。可用性:UNIX。

### asyncore 实例
一个http client的实现。
```python
import socket
import asyncore


class Client(asyncore.dispatcher):
    
    def __init__(self, host, path):
        asyncore.dispatcher.__init__(self)
        self.create_socket(socket.AF_INET, socket.SOCK_STREAM)
        self.connect((host, 80))
        self.buffer = 'GET %s HTTP/1.0\r\n\r\n' % path

    def handle_connect(self):
        pass

    def handle_close(self):
        self.close()

    def handle_read(self):
        print self.recv(8192)

    def writable(self):
        return (len(self.buffer) > 0)

    def handle_write(self):
        sent = self.send(self.buffer)
        self.buffer = self.buffer[sent:]

client = Client('www.python.org', '/')
asyncore.loop()
```
服务器接受连接和分配任务
```python
import socket
import asyncore


class EchoHandler(asyncore.dispatcher_with_send):
    
    def handle_read(self):
        data = self.recv(8192)
        if data:
            self.send(data)


class EchoServer(asyncore.dispatcher):
    
    def __init__(self, host, port):
        asyncore.dispatcher.__init__(self)
        self.create_socket(socket.AF_INET, socket.SOCK_STREAM)
        self.set_reuse_add()
        self.bind((host, port))
        self.listen(5)

    def handle_accept(self):
        pair = self.accept()
        if pair is not None:
            sock, addr = pair
            print 'Incoming connection from %s' % repr(addr)
            handler = EchoHandler(sock)

server = EchoServer('localhost', 8080)
asyncore.loop()
```
