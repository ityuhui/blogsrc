title: Python 装饰器模式

date: 2015-03-22 20:15:47

categories:

- Python

tags:

- python
- 装饰器
- 设计模式
- Design Patterns

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

如果只是在被装饰的函数之前加入一些功能，装饰器函数不用设计成嵌套的，函数末尾返回被装饰的函数即可，这种用法不太常见。例如：

```python
registry = []

def register(func):
     print('running register (%s)' % func)
     registery.append(func)
     return func

@register
def f1():
     print('running f1()')
```

如果要修改被装饰的函数的功能，装饰器函数要定义成嵌套函数，并且返回内嵌套函数。例如：

```python
def clock(func):
     def clocked(*args):
          t0 = time.perf_counter()
          result = func(*args)
          ecapsed = time.perf_counter() - t0
          print('ecapsed=%f' % ecapsed)
          return result
     return clocked

@clock
def f1():
     print('running f1()')
```

还有高级的用法，生成装饰器的工厂：

```python
DEFAULT_FMT= '[{elapsed: 0.8f}s]{name}({args})->{result}'
def clock(fmt=DEFAULT_FMT):  # 装饰器工厂
     def decorate(func):     # 真正的装饰器
          def clocked(*_args):
               t0 = time.time()
               _result = func(*_args)
               elapsed = time.time() - t0
               nmae = func.__name__
               args = ','.join(repr(arg) for arg in _args)
               result = repr(_result)
               print(fmt.format(**locals()))
               return _result
          return clocked
     return decorate

if __name__ == '__main__':
     @clock()  # 这里是关键，必须加上括号()，表示要调用这个函数执行，返回装饰器
     def snooze(seconds):
          time.sleep(seconds)
```

装饰器可以叠放

```python
@c2
@c1
def f1():
     pass
```

相当于

```python
f = c2(c1(f1))
```

## 参考

- 《流畅的 Python》
- <http://www.cnblogs.com/PandaBamboo/archive/2013/01/17/2865003.html>
