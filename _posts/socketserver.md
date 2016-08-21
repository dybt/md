title: python SocketServer 模块
date: 2015-07-24 13:51:53
categories: python
tags: [python, 标准库]
---
`SocketServer`创建一个网络服务框架。它定义了类来处理`TCP`,`UDP`, `UNIX streams` 和`UNIX datagrams`上的同步网络请求。
### Server Types
有五个不同的服务器类在`SocketServer`中。
>`BaseServer`定义了API, 而且他不是用来实例化和直接使用的。
>`TCPServer`用作`TCP/IP`的socket通讯。
>`UDPServer`使用datagram sockets。
>`UnixStreamServer`和`UnixDatagramServer`使用Unix-domain sockets而且智能在unix平台上使用。

### Server Objects
构建一个服务器, 通过它来监听请求的地址和请求的处理类(not instance)。
>class SocketServer.BaseServer
>这是模块中所有服务器对象的超类，它定义了接口,实现大多数都在子类中完成。

#### BaseServer.fileno
>返回一个整数文件描述符来表示哪个服务器正在监听。这个函数最常见的传递给`select.select()`，允许监控多个相同处理过程的服务。

#### BaseServer.handle_request
>处理单一的请求,这个函数会顺序调用接下来的方法。`get_request()`,`verify_request`和`proccess_request`。
>用户提供`handle()`方法抛出一个异常，那么`handle_error()`方法会被调用。
>`self.timeout`的时间内没有收到请求，`handle_timeout()`和`handle_request()`将返回。

<!--more-->
#### BaseServer.serve_forever
>BaseServer.serve_forever(poll_interval=0.5)，处理请求一直到明确的`shutdown()`请求。轮训每隔`poll_interval`时间内关闭。忽略`self.timeout`,如果需要使用定时任务，需要使用其他线程。

#### BaseServer.shutdown
>告诉`serve_forever()`循环停止。

#### BaseServer.RequestHandlerClass
>用户请求处理程序类，为每个请求创建这个类的一个实例。

### Implementing a Server
如果你创建一个服务器，它通常可以重复使用现有的类和简单的提供一个自定义请求处理的类。如果不符合需求，有几种BaseServer方法覆盖一个子类。

* verify_request(reqeust, client_address): 必须返回一个布尔值，如果返回`True`,请求将被处理，如果返回`False`，请求将被拒绝。这个函数可以覆盖来实现访问控制服务。
* process_request(request, client_address): 调用`finish_request`来创建一个`RequestHandlerClass()`的实例，如果需要该函数可以创建一个新的进程或协程来处理请求。
* finish_request(request, client_address): 创建一个请求处理实例。调用`handle()`来处理请求。

### Request Handlers
请求处理程序做的大部分工作接收传入的请求,并决定采取何种行动。处理程序负责实现“协议”上的套接字层(例如,HTTP或xml - rpc)。从传入的请求处理程序读取请求数据通道,流程,和写一个响应。有三个方法可以重写。

* setup(): 准备请求的请求处理程序, 就是初始化运行在handle之前。
* handle(): 做真正的请求工作。解析传入的请求，处理数据和返回响应。
* finish(): 清理任意时间创建的`setup()`。

### 例子
下面例子展示了`tcp`, `udp`和　异步

#### TCPServer 例子
```python
import SocketServer


class MyHandler(SocketServer.BaseRequestHandler):
    
    def handle(self):
        self.data = self.request.recv(1024).strip()
        print '{} wrote:'.format(self.client_address[0])
        print self.data
        self.request.sendall(self.data.upper())


if __name__ == '__main__':
    HOST, PORT = 'localhost', 9999
    server = SocketServer.TCPServer((HOST, PORT), MyHandler)
    server.serve_forever()
```
#### UDPServr 例子
```python
import SocketServer


class MyHandler(SocketServer.BaseRequestHandler):

    def handle(self):
        data = self.request[0].strip()
        socket = self.request[1]
        print '{} wrote:'.format(self.client_address[0])
        print data
        socket.sendto(data.upper(), self.client_address)


if __name__ == '__main__':
    HOST, PORT = 'localhost', 9999
    server = SocketServer.UDPServer((HOST, PORT), MyHandler)
    server.serve_forever()
```
#### 异步例子
可以通过`ThreadingMixIn`和`ForkingMixIn`类来构造异步处理程序。
```python
import socket
import threading
import SocketServer


class MyHandler(SocketServer.BaseRequestHandler):

    def handle(self):
        data = self.request.recv(1024)
        curr_thread = threading.current_thread()
        response = '{}: {}'.format(curr_thread.name, data)
        self.request.sendall(response)


class Server(SocketServer.ThreadingMixIn, SocketServer.TCPServer):
    pass


def client(ip, port, message):
    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    sock.connect((ip, port))
    try:
        sock.sendall(message)
        response = sock.recv(1024)
        print 'Received: {}'.format(response)
    finally:
        sock.close()


if __name__ == '__main__':
    HOST, PORT = 'localhost', 0

    server = Server((HOST, PORT), MyHandler)
    ip, port = server.server_address

    serer_thread = threading.Thread(target=server.serve_forever)
    server_thread.daemon = True
    server_thread.start()
    print 'Server loop running in thread:', server_thread.name

    client(ip, port, 'Hello World 1')
    client(ip, port, 'Hello World 2')
    client(ip, port, 'Hello World 3')

    server.shutdown()
    server.server_close()
```
### 参考文献
1. [pymotw-socketserver](http://pymotw.com/2/SocketServer/index.html#module-SocketServer)
2. [pylib-socketserver](https://docs.python.org/2/library/socketserver.html)
