---
title: "如何理解Python中的装饰器"
date: 2022-08-08T01:01:02+08:00
draft: false
tags:
- Python
---

装饰器来自 Decorator 的直译，就是装点、提供一些额外的点缀。
在 Python 中的装饰器则是提供了一些额外的功能，可以让你的函数在不改变代码的情况下拥有新的功能。

首先，Python中的函数也是一个对象，这意味着函数：
- 能在函数中定义一个函数
- 能作为参数传递
- 能作为返回值

```
def decorator(func):
    def wrapper(*args, **kwargs):
        print('123')
        return func(*args, **kwargs)

    return wrapper

def say_hello():
    print('同学你好')

say_hello_super = decorator(say_hello)
say_hello_super()
```

```
123
同学你好
```

在以上这段代码中，我们将一个函数 say_hello 作为参数传入函数 decorator，返回一个 wrapper 函数并赋值到 say_hello_super，此时执行 say_hello_super 相当于执行 wrapper 函数。当我们执行 wrapper 函数时会先打印 123 再执行先前传入的 func 参数也就是 say_hello 函数。

注意 wrapper 的 *args 与 **kwargs 参数，这是必须的， *args 表示所有的位置参数，**kwargs 表示所有的关键字参数。之后再将其传到 func函数中， 这样保证了能完全传递所有参数。

在这里，decorator 这个函数就是一个装饰器，功能是在执行被装饰的函数之前打印 123。

在 python 中， 有一种语法糖可以代替 say_hello_super = decorator(say_hello) 这一步的操作，以上的代码可以改写成。

```
@decorator
def say_hello():
    print('同学你好')

say_hello()
```

这里在函数前加上 @decorator 相当于在定义函数后执行了一条语句， say_hello = decorator(say_hello) 。

## 带参数的装饰器

之前的装饰器是在每次执行函数前打印 123， 如果我们想指定打印的值，那该怎么办？

```
def info(value):
    def decorator(func):
        def wrapper(*args, **kwargs):
            print(value)
            return func(*args, **kwargs)

        return wrapper

    return decorator

@info('456')
def say_hello():
    print('同学你好')

say_hello()
```

```
456
同学你好
```

我们可以在装饰器外部再套上一层函数，用该函数的参数接收我们想要打印的数据，并将先前的 decorator 函数作为返回值。这就是之前学到的闭包的一种功能，就是用闭包来生成一个命名空间，在命名空间中保存我们要打印的值 value。

## wraps 装饰器

一个函数不止有他的执行语句，还有着 name__（函数名），__doc （说明文档）等属性，我们之前的例子会导致这些属性改变。

```
def decorator(func):
    def wrapper(*args, **kwargs):
        """doc of wrapper"""
        print('123')
        return func(*args, **kwargs)

    return wrapper

@decorator
def say_hello():
    """doc of say hello"""
    print('同学你好')

print(say_hello.__name__)
print(say_hello.__doc__)
```

```
wrapper
doc of wrapper
```

由于装饰器返回了 wrapper 函数替换掉了之前的 say_hello 函数，导致函数名，帮助文档变成了 wrapper 函数的了。

解决这一问题的办法是通过 functools 模块下的 wraps 装饰器。

```
from functools import wraps

def decorator(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        """doc of wrapper"""
        print('123')
        return func(*args, **kwargs)

    return wrapper

@decorator
def say_hello():
    """doc of say hello"""
    print('同学你好')

print(say_hello.__name__)
print(say_hello.__doc__)
say_hello
doc of say hello
```

> 参考资料:  
> [如何理解Python装饰器？ - 三眼鸭的编程教室的回答](https://www.zhihu.com/question/26930016/answer/1904166977)  
>  [Python 装饰器执行顺序迷思](https://segmentfault.com/a/1190000007837364)