---
title: "Python用前需知"
date: 2022-09-29T13:54:09+08:00
draft: false
categories:
- 
tags:
- Configuration
- Python
searchHidden: false
---

## pip

### pip常用命令

```
pip install
pip list
```

### pip换源

#### 临时使用

```-i``` 命令指定下载源
```
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple some-package
```

#### 命令行全局切换

要求pip版本>=10.0.0

```
python -m pip install --upgrade pip
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```

如果升级pip时默认源的网络连接较差，可以利用刚才的```-i```临时指定下载源
```
python -m pip install -i https://pypi.tuna.tsinghua.edu.cn/simple --upgrade pip
```

#### 国内镜像库列表

```
https://pypi.tuna.tsinghua.edu.cn/simple
https://mirrors.aliyun.com/pypi/simple/
https://pypi.mirrors.ustc.edu.cn/simple
http://pypi.douban.com/simple/
http://mirrors.aliyun.com/pypi/simple/
```

## 虚拟环境

## 

> 参考资料:  
> [PyPI 镜像使用帮助](https://mirrors.tuna.tsinghua.edu.cn/help/pypi/)