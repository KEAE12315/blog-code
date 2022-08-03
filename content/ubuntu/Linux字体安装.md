---
title: "Linux字体安装"
date: 2023-03-10T12:13:45+08:00
draft: true
categories:
- 
tags:
- 
searchHidden: false
---

## 字体文件

## 查看

```
fc-list :lang=zh
```

字体文件目录

在 GNU/Linux 中，字体可以在多个位置保存。这些位置在 /etc/fonts/fonts.conf 中定义;标准的包括 /usr/share/fonts、/usr/local/share/fonts 和 ~/.fonts。44 安装 truetype 字体的最简单方法是按 alt-F2 并输入以下代码（这将在正确的目录中打开 nautilus）：

sudo fc-cache -f -v

## 参考

- [Ubuntu wiki](https://wiki.ubuntu.com/Fonts)
- [How to install new fonts on Linux](https://www.internalpointers.com/post/install-new-fonts-linux-command-line)