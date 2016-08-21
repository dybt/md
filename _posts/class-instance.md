title: 类和实例
date: 2015-04-11 16:34:55
categories: python
tags: [python, 类]
---
python是一个面向对象的语言,而面向对象最重要的概念就是类和实例, 记得刚学习的时候不太理解这些概念,直到老师说了一句"物以类聚". 没错就是类, 归类
### 物以类聚
类其实就是把一些相同特性的事物归成一类, 比如人
```python    
    class Person(object):
        pass
```
<!--more-->
我们定义了**人**这个类, 但人有一些特性,比如 两个眼睛,一个嘴巴, 我们把这些添加进去
```python
    class Person(object):
        eyes = 2
        mouth = 1
```

已经把人的一些信息写进去了,但是人还有名字, 比如我**mink**. 好吧我不能亏待自己我得把这些添加进去
```python  
    class Person(object):
        eyes = 2
        mouth = 1
        name = mink
```
太完美了,一个人终于完成了. 上帝用了一天我就用了一分钟(开个玩笑), 我们读一下信息. 人类他有两个眼睛,一个嘴巴,还有名字叫mink. - -! 有点不对,mink是我的名字啊~ 怎么人类叫mink呢 
mink是人类的名字, 人类的名字是mink显然是错误的, "wo" 应该是人类的个体,是个单个例子 
```python   
class Person(object):
    eyes = 2
    mouth = 1
    
    def __init__(self, name):
        self.name = name 

me = Person('mink')
```

现在我终于有了自己的名字而不是给大家共用, 这个方法叫实例但是我有一个别人不会的技能, **我不受重力影响**.

```python
class Person(object):
    eyes = 2
    mouth = 1

    def __init__(self, name)
        self.name = name 

    def jineng(self, txt):
        print "%s %s" % (self.name, txt)

me = Person('mink')
me.jineng("我不受重力影响, 我会飞")
```
