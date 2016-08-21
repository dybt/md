title: python 协程
date: 2015-06-09 10:50:36
categories: python
tags: [协程, python]
---
在单线程环境下为了自己控制上下文就有了协程的概念.

### 一个简单协程
<hr>
将通过生产者消费者试一下协程的特性

```python
    import time

    def corontine(func):
        def _deco():
            ret = func()
            ret.next()
            return ret
        return _deco


    @corontine
    def consumer():
        while True:
            n = yield 1
            print '消费 %s' % n


    def producer(c):
        n = 0
        while True:
            n += 1
            print '生产 %s' % n
            r = c.send(n)
            print '消费者 返回', r

    if __name__ == '__main__':
        c = consumer()
        producer(c)
```

<!--more-->
这段代码通过`生成器`简单实现了协程.其中`producer`生产后通过`c.send`将n传给`consumer`，然后在通过`yield`接受`consumer`返回的1.

### greenlet 协程库
<hr>
一个`greenlet`的文档中的例子
```python
    from greenlet import greenlet

    def test1():
        print 12
        gr2.switch()
        print 34

    def test2():
        print 56
        gr1.switch()
        print 78

    gr1 = greenlet(test1)
    gr2 = greenlet(test2)
    gr1.switch()
```

这个例子会打印出 12 56 34 其中通过`switch`来调度任务

#### greenlet 实例化
`greenlet.greenlet` 是一个`greenlet`类型.

*greenlet(run=None, parent=None)*
>创建一个新的`greenlet`对象. `run`是一个可调用的*callable*.`parent`是一个*parent greenlet*.

 *greenlet.getcurrent()*       
 >返回一个当前的`greenlet`.

 *greenlet.GreenletExit*
 >用来杀死单一`greenlet`.

 #### Switching
 `switch`用来调用函数.

```python
    def test1(x, y):
        z = gr2.switch(x+y)
        print z

    def test2(u):
        print u
        gr1.switch(42)

    gr1 = greenlet(test1)
    gr2 = greenlet(test2)
    gr1.switch('hello', 'world')
```

打印出 hello world,  42。`gr1.switch`会把`hello world`传给gr2并切换到gr2打印出u然后通过gr1.switch在切换到gr1.

**g.switch(\*args, \*\*kwargs)**
>开关执行`greenlet`对象`g`, 把这个对象传给参数。特殊情况下, 如果`g`没有开启，那么立即执行.
>`x = g.switch(y)` 会把`y`传给`g`对象，switch执行后会展厅，当`g`返回对象时会接受.

#### greenlet 方法和属性

**g.switch(\*args, \*\*kwargs)**
>执行`greenlet`类型`g`

**g.run**
>调用`g`并运行, 如果`g`开启则这个属性将不存在.

**g.parent**
>`greenlet`父对象.是个可写的，但不允许周期内创建父对象.

**g.gr_frame**
>当前的顶层结构或`None`

**g.dead**
>如果`g`销毁则`True`.

**bool(g)**
>如果`g`执行中则`True`。如果`g`销毁或没有执行则`False`

**g.throw([typ, [val, [tb]]])**
>执行`g`但会跑出给定的异常，如果没有提供参数，默认`greenlet.GreenletExit`与下面例子相似:

```python
    def raiser():
        raise typ, val, tb

    g_raiser = greenlet(raiser, parent=g)
    g_raiser.switch()
```
greenlets and python threads
>greenlets 与python threads结合。每个线程都包含一个独立的`greenlet`不同的线程之间是不能切换和混合`greenlets`的。


