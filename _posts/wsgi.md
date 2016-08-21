title: WSGI 协议
date: 2015-11-28 14:36:59
category: python
tags: [python, wsgi]
---
**维基百科**: WSGI是(Python Web Server Gateway Interface)的缩写。是为Python语言定义Web服务器和Web应用框架的简单而通用的接口。

### 实现目的
Python实现了很多服务器和框架, 但是因为两者不兼容，所以选择一个框架或服务器的时候不能自由的选择另一个。为了解决Python服务器和框架之间的兼容性, WSGI协议诞生了。WSGI规定框架和服务器之间的接口规范,　使得服务器和框架之间的互通.

### Web框架
一个web应用对象是一个可调用对象,接受两个参数。这里的对象并不能误解为一个对象的实例：一个函数，方法，类和含有(\_\_call\_\_)方法的实例都是可接受的应用对象。应用对象必须能够调用多次, 因为所有的(服务器/网关)都会发出重复请求。

下面有两个应用程序实例; 一个是函数方式，另一个是类的方式实现。
```python
def simple_app(envrion, start_response):
    status = '200 OK'
    response_headers = [('Content-type', 'text/plain')]
    start_response(status, response_headers)
    return ['Hello world!\n']


class AppClass:
    def __init__(self, environ, start_response):
        self.environ = environ
        self.start = start_response

    def __iter__(self):
        status = '200 OK'
        response_headers = [('Content-type', 'text/plain')]
        self.start(status, response_headers)
        yield "Hello world!\n"
```

<!--more-->

### Web服务器与网关
在HTTP客户端发送一次请求时，服务器和网关都会调用一次的应用(callable)。

```python
import socket
import StringIO
import sys

class Server(objects):

    def __init__(self, server_address):
        self.listen_socket = listen_socket = socket.socket(socket.AF_INET,
                                                           socket.SOCK_STREAM)
        listen_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
        listen_socket.bind(server_address)
        listen_socket.listen(5)

        host, port = self.listen_socket.getsockname()[:2]
        self.server_name = socket.getfqdn(host)
        self.server_port = port

        self.header_set = []

    def set_app(application):
        # 向服务器加载应用
        self.application = application

    def server_forever(self):
        listen_socket = self.listen_socket
        while True:
            # 循环接受客户端请求
            self.client, client_address = listen_socket.accept()
            
            # 处理单个请求
            self.handle_one_request()

    def handle_one_request(self):
        self.request_data = request_data = self.client.recv(1024)
        print(''.join(
                '< {line}\n'.format(line=line)
                for line in request_data.splitlines()))

        # 解析请求数据
        self.parse_request(request_data)
        # 在服务器提供的环境变量信息
        env = self.get_environ()

        # 向框架传递两个参数，第一个为环境变量，第二个是回调函数.
        result = self.application(env, self.start_response)
        
        self.finish_response(result)

    def parse_request(self, text):
        request_line = text.splitlines()[0]
        request_line = request_line.rstrip('\r\n')
        (self.request_method,
         self.path,
         self.request_version) = request_line.split()

    def get_environ(self):
        env = {}
        env['wsgi.version'] = (1, 0)
        env['wsgi.url_scheme'] = 'http'
        env['wsgi.input'] = StringIO.StringIO(self.request_data)
        env['wsgi.errors'] = sys.stderr
        env['wsgi.multithread'] = False
        env['wsgi.multiprocess'] = False
        env['wsgi.run_once'] = False

        env['REQUEST_METHOD'] = self.request_method
        env['PATH_INFO'] = self.path
        env['SERVER_NAME'] = self.server_name
        env['SERVER_PORT'] = str(self.server_port)
        return env

    def start_reponse(self, status, response_headers, exc_info=None):
        server_headers = [
            ('Server', 'WSGIServer 0.2')
        ]
        self.headers_set = [status, response_headers + server_headers]

    def finish_response(self, result):
        try:
            status, response_headers = self.headers_set
            response = 'HTTP/1.1 {status}\r\n'.format(status=status)
            for header in response_headers:
                response += '{0}: {1}\r\n'.format(*header)
            response += '\r\n'
            for data in result:
                response += data
            print(''.join(
                '> {line}\n'.format(line=line)
                for line in response.splitlines()))
            self.client.sendall(response)
        finally:
            self.client.close()

SERVER_ADDRESS = (HOST, PORT) = '', 5000


def make_server(server_address, application):
    server = WSGIServer(server_address)
    server.set_app(application)
    return server

if __name__ == '__main__':
    if len(sys.argv) < 2:
        sys.exit('Provide a WSGI application object as module:callable')
    app_path = sys.argv[1]
    module, application = app_path.split(':')
    module = __import__(module)
    application = getattr(module, application)
    httpd = make_server(SERVER_ADDRESS, application)
    print('WSGIServer: Serving HTTP on port {port} ...\n'.format(port=PORT))
    httpd.server_forever()
```

### environ 变量
environ字典需要包含CGI environments变量。作为公共网关接口规范定义。下列变量必须存在，除非他们的值是空的字符串。

---
#### REQUEST_METHOD
HTTP 请求方式，比如"GET"或"POST"。这个参数不能是空字符串，所以总是必须的。

---
#### SCRIPT_NAME
请求URL的开始部分,对应于应用程序对象，所以应用程序知道其虚拟"位置"。如果应用程序对应服务器根目录，`SCRIPT_NAME`可能为空字符串。

---
#### PATH_INFO
请求URL的剩余部分，指定请求的虚拟"位置"的目标在应用程序中。如果请求URL的目标是应用程序跟目录并且末尾没有砍掉的话, `PATH_INFO`可能为空字符串。

---
#### QUERY_STRING
请求URL的`?`之后的部分, 可以为空或不存在。

---
#### CONTENT_TYPE
HTTP请求中Content-Type字段包含的内容，可以为空或不存在。

---
#### CONTENT_LENGTH
HTTP请求中Content-Length字段包含的内容，可以为空或不存在。

---
#### SERVER_NAME, SERVER_PORT
当结合`SCRIPT_NAME`和`PATH_INFO`时这些变量可以使用完整的URL。如果存在HTTP_HOST，那么重建请求时优先使用HTTP_HOST。SERVER_NAME和SERVER_PORT从未是空的，所以他们总是必须的。

---
#### SERVER_PROTOCOL
客户端发送请求的协议。通常是类似"HTTP / 1.0"或"HTTP / 1.1", 课时使用应用程序来决定如何对待任何HTTP请求头。(这个变量应该被成为REQUEST_PROTOCOL, 因为它表示请求中使用的协议，而不一定用在服务器响应中, 然而对于兼容的CGI我们必须保持现在的名字。)

---
#### HTTP_Variables
这组变量对应客户端提供的HTTP请求包头。这些变量的存在与否都应与HTTP报头保持一致。

除了CGI变量，environ字典可以包含任意操作系统"环境变量", 而且必须包含下面`WSGI`变量。

变量|值
---|---
wsgi.version|元组(1, 0), 代表WSGI版本。
wsgi.url_scheme|应用程序被调用过程中的一个字符串，通常是"http"或"https"。
wsgi.input|一个可以被HTTP请求体读取的输入流(类文件对象)。
wsgi.errors|输出流(类文件对象)可以编写错误输出,为了标准化和集中记录程序或其他错误。
wsgi.multithread|如果有其他线程同时调用这个应用程序对象。则参数值应该为`True`,否则返回`False`。
wsgi.multiprocess|如果有其他进程同时调用这个应用程序对象。则参数值应该为`True`,否则返回`False`。
wsgi.run_once|如果服务器和网关预计(但不保证)应用程序在他的生命周期内只被调用一次.则参数值应该为`True`，否则返回`False`。通常这只对给予CGI的网关。

最后`environ`字典也可能包含服务器定义的变量。这些变量名应该只使用小写字母，数字点和下划线，并且变量名前缀应该使用唯一的服务器和网关。例如，mod_python可以定义mod_python.some_varible。

#### 输入流和错误流
服务器提供的输入流和错误流中必须支持下面的方法：

方法|流|说明
---|---|---
read(size)|input|1
readline()|input|1,2
readlines(hint)|input|1,3
__iter__()|input|
flush()|errors|4 
write(str)|errors|
writelines(seq)|errors|

1. 服务器不需要读取超过客户端指定的`Content-Length`, 如果应用试图读取超过的设置的点，服务器应该模拟一个终止条件。应用不应该试图读取超过指定`CONTENT_LENGTH`长度的数据。
2. 可选的`size`参数在`readline()`中不被支持，因为可能对服务器作者实现增加复杂度，而且在实践中并不经常使用。
3. 注意，`hint`参数在readlines()是可选的，应用程序可以选择不提供它，服务器和网关也可以忽略他。
4. 由于错误流不能倒回，服务器和网关可以自由提出立即写操作，没有缓冲。在这种情况下，`flush()`方法可能是一个空操作。不过一个便捷式应用程序，不能假定无缓冲输出和`flush()`是一个空操作。如果他们需要保证输出一直写就必须调用`flush()`。

上诉表中列出的方法必须由所有服务器支持符合这个规范。应用程序不能对输入流和错误流使用不符合此规范的方法或属性。特别的，应用程序必须不试图关闭这些流，计时他们拥有`close()`方法。

### 回调函数 start_response()
应用程序对象的第二个参数是回调函数，类似于start_reponse(status, reponse_headers, exc_info=None)。(参数是按顺序排序，而不是键值)，必须返回`write(body_data)`的callable。

status 参数是一个HTTP的`状态`字符串，像"200 OK"或"404 Not Found"。它是一个由状态码和状态信息短语组成的字符串，依次由空格分开两边美柚空格或其他字符。字符串必须不包含控制字符，不能是回车换行或其他组合符号。

response_headers 参数一个元组(header_name, header_value)列表。它必须是一个Pyhton列表,即type(response_headers)是一个列表类型，服务器可以修改它的内容，但是每一个`header_name`必须是合法的HTTP header字段名[(参见RFC 2616, 章节4.2)](http://www.faqs.org/rfcs/rfc2616.html)，末尾不能有冒号或其他标点符号。

每个`header_value`不能包含任意控制符，如回车或换行等。(这些要求用来减少服务器和网关解析响应头的复杂度。)

一般来说服务器或网关确保正确的头被发送到客户端:如果应用程序忽略了所需的HTTP header那么服务器必须给它加上。

#### 处理 Content-Length Header
如果应用程序没有提供Content-Length头信息，服务器可以有集中处理方式，最简单的就是响应完成的时候关闭客户端连接。


### 参考
1. [PEP 0333](https://www.python.org/dev/peps/pep-0333/#the-application-framework-side)
2. [伯乐在线](http://python.jobbole.com/81523/)

