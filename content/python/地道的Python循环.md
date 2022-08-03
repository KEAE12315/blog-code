---
title: "地道的Python循环"
date: 2022-08-08T01:05:13+08:00
draft: false
tags:
- Python
---

在循环语法方面，Python 选择了 for 和 while 这两个经典的关键字来表达循环。绝大多数情况下，我们的循环需求都可以用 for <item> in <iterable> 来满足，while <condition> 相比之下则用的更少些。

Python 的 for 循环只有 for <item> in <iterable> 这一种结构，而结构里的前半部分 - 赋值给 item - 没有太多花样可玩。所以后半部分的 可迭代对象 是我们唯一能够大做文章的东西。而以 enumerate() 函数为代表的“修饰函数”，刚好提供了一种思路：通过修饰可迭代对象来优化循环本身。

## 使用函数修饰被迭代对象来优化循环

内置模块 itertools 是一个包含很多面向可迭代对象的工具函数集。 Python 官方文档是学习的首选，里面有非常详细的模块相关资料。下面给出一些常用的函数。

- enumerate(iterable)：返回元素和下标
- product(A, B)：返回A, B的笛卡儿积
- islice(seq, start, end, step)：循环内隔行处理
- takewhile(predicate, iterable)：替代 break 语句
- zip(A, B)：按顺序同时迭代A，B

## 按职责拆解循环体内复杂代码块

写循环时，我们很容易往循环体塞入越来越多的代码，包括过滤掉无效元素、预处理数据、打印日志等等，甚至一些原本不属于同一抽象的内容，导致各部分功能耦合。

这样一个复杂循环体在面对新需求时难以解耦复用。考虑使用生成器函数解耦循环体。

> 参考资料:  
> [Python 工匠：编写地道循环的两个建议](https://segmentfault.com/a/1190000042311321)