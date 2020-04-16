title: Python装饰器模式

date: 2015-03-22 20:15:47

categories:
- Python

tags:
- python
- 装饰器
---

```python
@deco
def foo():
     pass
```

<!--more-->

其实@只是个语法糖, 可以翻译成
```python
foo=deco(foo)
```

deco函数要定义成嵌套函数，并且返回内嵌套函数，这样才能修改foo

[参考](http://www.cnblogs.com/PandaBamboo/archive/2013/01/17/2865003.html)
