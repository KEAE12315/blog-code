---
title: "Python文件处理的常用方法"
date: 2022-08-08T00:56:36+08:00
draft: false
tags:
- Python
---

## 遍历文件或目录

我们常常需要对数据集文件夹中的数据进行遍历，Python内置的 os 模块有很多有用的方法能被用来列出目录内容和过滤结果。

### 常用函数介绍

#### os.listdir(path)

返回一个list，其中包含path参数所指目录的文件和子目录的名称

#### os.walk(top[, topdown=True[, onerror=None[, followlinks=False]]])

- top – 返回值，为一个三元组(root,dirs,files)。
- root 所指的是当前正在遍历的这个文件夹的本身的地址
- dirs 是一个 list ，内容是该文件夹中所有的目录的名字(不包括子目录)
- files 同样是 list , 内容是该文件夹中所有的文件(不包括子目录)
- topdown –可选，控制遍历是从上到下，还是反过来。 为True优先遍历上层目录，否则优先遍历最底层目录(默认为True)。
- onerror – 可选，需要一个 callable 对象，当 walk 需要异常时，会调用。
- followlinks – 可选，如果为 True，则会遍历目录下的快捷方式(linux 下是软连接 symbolic link )实际所指的目录(默认关闭)，如果为 False，则优先遍历 top 的子目录。

示例代码:

```
for root,dirs,files in os.walk(mypath):
  print(root)
  for dr in dirs:
      print(dr)
  for name in files:
      if name.endswith(".txt"):
          print(os.path.join(root, name))
```

排序问题

需要注意的是os.walk使用os.listdir，两者返回的列表都是任意顺序的，如果想要获得排序，考虑使用sort。例如：

```
# 如果要按排序顺序递归目录，则就地修改dirs
for root, dirs, files in os.walk(path):
   dirs.sort()
   for dirname in dirs:
        print(os.path.join(root, dirname))

# 如果要按数字顺序列出它们，请使用:
for dirname in sorted(dirs, key=int):

# 要对字母数字字符串进行排序，请使用natural sort。
```

> 参考资料:  
> [Working With Files in Python](https://realpython.com/working-with-files-in-python/)  
> [Python文件操作，看这篇就足够](https://zhuanlan.zhihu.com/p/56909212)  
> [In what order does os.walk iterates iterate?](https://stackoverflow.com/questions/18282370/in-what-order-does-os-walk-iterates-iterate)