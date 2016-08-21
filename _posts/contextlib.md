title: python contextlib 模块
date: 2015-07-13 17:46:13
categoeis: python
tags: [python, 标准库]
---
contextlib模块提供使用工具用于处理上下文和with声明。
### Context Manager API
一个上下文管理器通过`with`声明激活, 而且API包含两个方法。`__enter__()`方法运行执行流进入到`with`代码块内。他返回一个对象共上下文使用。当执行流离开`with`块时，`__exit__()`方法上下文管理器清除任何资源被使用。
```python
class Context(object):
    
    def __init__(self):
        print '__init__()'

    def __enter__(self):
        print '__enter__()'
        return self

    def __exit__(self, exc_type, exc_val, exc_tb):
        print '__exit__()'

with Context():
    print 'Doing work in the context.'
```
打印结果
```python
__init__()
__enter__()
Doing work in the context.
__exit__()
```
执行上下文管理器时会调用`__enter__`离开时调用`__exit__`。
<!--more-->
`__enter__`能返回任意对象，联合一个指定名字于`with`声明。
```python
class WithinContext(object):

    def __init__(self, context):
        print 'WithinContext.__init__(%s)' % context

    def do_something(self):
        print 'WithinContext.do_something()'

    def __del__(self):
        print 'WithinContext.__del__'


class Context(object):

    def __init__(self):
        print '__init__()'
   
    def __enter__(self):
        print '__enter__()'
        return WithinContext(self)
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        print '__exit__()'

with Context() as c:
    c.do_something()
```
打印结果
```python
__init__()
__enter__()
WithinContext.__init__(<__main__.Context object at 0x7f579d8e4890>)
WithinContext.do_something()
__exit__()
WithinContext.__del__
```
如果上下文管理器能处理异常,`__exit__()`应该返回一个`True`值表明这个异常不需要传播,返回`False`异常会在执行`__exit__`之后被引起。
```python
class Context(object):

    def __init__(self, handle_error):
        print '__init__(%s)' % handle_error
        self.handle_error = handle_error
    
    def __enter__(self):
        print '__enter__()'
        return self
   
    def __exit__(self, exc_type, exc_val, exc_tb):
        print '__exit__(%s, %s, %s)' % (exc_type, exc_val, exc_tb)
        return self.handle_error

with Context(True):
    raise RuntimeError('error message handled')

print

with Context(False):
    raise RuntimeError('error message propagated')
```
打印结果
```python
__init__(True)
__enter__()
__exit__(<type 'exceptions.RuntimeError'>, error message handled, <traceback object at 0x7fdfb32f8b00>)

__init__(False)
__enter__()
__exit__(<type 'exceptions.RuntimeError'>, error message propagated, <traceback object at 0x7fdfb32f8b90>)
Traceback (most recent call last):
  File "test.py", line 23, in <module>
      raise RuntimeError('error message propagated')
      RuntimeError: error message propagated
```
### 从生成器到上下文管理器
创建上下文管理的传统方法,通过编写一个类与`__enter__()`和`__exit__()`方法,并不困难。但有时比你需要的开销只是管理一个微不足道的上下文。在这类情况下,您可以使用contextmanager() decorat or 生成器函数转换成一个上下文管理器。
```python
import contextlib

@contextlib.contextmanager
def make_context():
    print ' entering'
    try:
       yield {}
     except RuntimeError, err:
        print ' Error:', err
    finally:
        print ' exiting'
        
print 'Normal:'

with make_context() as value:
    print ' inside with statement:', value
    
print
print 'handled ereor:'

with make_context() as value:
    raise RuntimeError('show example of handling an error')

print
print 'unhandled error:'

with make_context() as value:
    raise ValueError('this exception is not handled')
```
打印结果
```python
Normal:
 entering
  inside with statement: {}
   exiting

handled ereor:
entering
 Error: show example of handling an error
  exiting

unhandled error:
entering
exiting
Traceback (most recent call last):
  File "test.py", line 30, in <module>
      raise ValueError('this exception is not handled')
      ValueError: this exception is not handled
```
### 嵌套上下文
使用`nested()`可以同时管理多个上下文。
```python
import contextlib

@contextlib.contextmanager
def make_context(name):
    print 'entering:', name
    yield name
    print 'exiting:', name

with contextlib.nested(make_context('A'), make_context('B'), make_context('C')) as (A, B,     C):
    print 'inside with statement:', A, B, C
```
打印结果
```python
entering: A
entering: B
entering: C
inside with statement: A B C
exiting: C
exiting: B
exiting: A
```
因为`Python 2.7`和以后的版本不赞成使用`nested()`,因为可以直接嵌套
```python
import contextlib

@contextlib.contextmanager
def make_context(name):
    print 'entering:', name
    yield name
    print 'exiting:', name

with make_context('A') as A, make_context('B') as B, make_context('C') as C:
    print 'inside with statement:', A, B, C
```
### 关闭open的句柄
文件类支持上下文管理器, 但是有一些对象不支持。还有一些类使用`close()`方法但是不支持上下文管理器。我们使用`closing()`来为他创建一个上下文管理器。(类必须有close方法)
```python
import contextlib


class Door(object):
    def __init__(self):
        print '  __init__()'
        
    def close(self):
        print '  close()'

print 'Normal Example:'
with contextlib.closing(Door()) as door:
    print '  inside with statement'
   
print 
print 'Error handling example:'
try:
    with contextlib.closing(Door()) as door:
        print '  raising from inside with statement'
        raise RuntimeError('error message')
except Exception, err:
    print '  Had an error:', err
```
打印结果
```python
Normal Example:
    __init__()
    inside with statement
    close()

Error handling example:
    __init__()
    raising from inside with statement
    close()
    Had an error: error message
```
### 参考文献
1. [pymotw-contextlib](http://pymotw.com/2/contextlib/index.html#module-contextlib)
