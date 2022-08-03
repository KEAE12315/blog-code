---
title: "Python程序计时"
date: 2022-08-08T00:51:32+08:00
draft: false
tags:
- Python
---

python的time内置模块是一个与时间相关的内置模块，存在不同的模块可以用于程序计时. 

## 使用time.time()

很多人喜欢用time.time()获取当前时间的时间戳，利用程序前后两个时间戳的差值计算程序的运行时间，如下：

```
import time
T1 = time.time()

#______假设下面是程序部分______
for i in range(100*100):
    pass

T2 = time.time()
print('程序运行时间:%s毫秒' % ((T2 - T1)*1000))
# 程序运行时间:0.0毫秒
```

不要以为你的处理器很厉害，就忽视了一个问题，一万次遍历，时间为0.0毫秒？

下面解决上面的质疑，

## 使用time.clock()

Python time clock() 函数以浮点数计算的秒数返回当前的CPU时间。用来衡量不同程序的耗时，比time.time()更有用。

这个需要注意，在不同的系统上含义不同。在UNIX系统上，它返回的是”进程时间”，它是用秒表示的浮点数（时间戳）。而在WINDOWS中，第一次调用，返回的是进程运行的实际时间。而第二次之后的调用是自第一次调用以后到现在的运行时间。（实际上是以WIN32上QueryPerformanceCounter()为基础，它比毫秒表示更为精确）

使用time.clock()更改后的程序查看一下：

```
import platform
print('系统:',platform.system())

import time
T1 = time.clock()

#______假设下面是程序部分______
for i in range(100*100):
    pass

T2 =time.clock()
print('程序运行时间:%s毫秒' % ((T2 - T1)*1000))
# 程序运行时间:0.27023641716203606毫秒
```

## 使用time.perf_counter()

返回性能计数器的值（以微秒为单位,1秒=1000毫秒；1毫秒=1000微秒）作为浮点数，即具有最高可用分辨率的时钟，以测量短持续时间。它包括在睡眠期间和系统范围内流逝的时间。返回值的参考点未定义，因此只有连续调用结果之间的差异有效。

1秒 = 1000毫秒

1毫秒 = 1000微秒

1微秒 = 1000纳秒

1纳秒 = 1000皮秒

import platform
print('系统:',platform.system())

```
import time
T1 = time.perf_counter()

#______假设下面是程序部分______
for i in range(100*100):
    pass

T2 =time.perf_counter()
print('程序运行时间:%s毫秒' % ((T2 - T1)*1000))
# 系统: Windows
# 程序运行时间:0.3007180604248629毫秒
```

## 使用time.process_time()

返回当前进程的系统和用户CPU时间总和的值（以小数微秒为单位）作为浮点数。

通常time.process_time()也用在测试代码时间上，根据定义，它在整个过程中。返回值的参考点未定义，因此我们测试代码的时候需要调用两次，做差值。

注意process_time()不包括sleep()休眠时间期间经过的时间。

```
import platform
print('系统:',platform.system())

import time
T1 = time.process_time()

#______假设下面是程序部分______
for i in range(100*100):
    pass

T2 =time.process_time()
print('程序运行时间:%s毫秒' % ((T2 - T1)*1000))
# 系统: Windows
# 程序运行时间:0.0毫秒
```

## 总结

建议PC上使用time.perf_counter() 来计算程序的运算时间，特别是测试算法在相邻两帧的处理时间，如果计算不准确，那可能会对算法的速度过于自信。

尤其在嵌入式的板子的开发中，性能的测试中，请仔细选择时间模块，比如某些嵌入式板子会封装专门的模块。

> 参考资料:  
> [python 利用time模块给程序计时](https://zhuanlan.zhihu.com/p/110005305)