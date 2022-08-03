---
title: "WSL2使用v2ray代理"
date: 2022-08-04T12:25:49+08:00
draft: false
tags:
- WSL
- Configuration
---

## 主机V2Ray设置

### 允许局域网连接

![zp](a.jpg)

### 获取监听端口

![](Snipaste-2022-08-04-14-41-13.jpg)

*对我而言, 有用的端口是局域网socks:10810*

## WSL2设置

### 配置环境变量

修改```.bashrc```文件, 添加以下内容(记得```source .bashrc```)
```
# v2ray proxy
export windows_host=`cat /etc/resolv.conf|grep nameserver|awk '{print $2}'`
export ALL_PROXY=socks5://$windows_host:10810
export HTTP_PROXY=$ALL_PROXY
export http_proxy=$ALL_PROXY
export HTTPS_PROXY=$ALL_PROXY
export https_proxy=$ALL_PROXY
```

### 测试连接情况

```
curl -vv google.com
```



> 参考资料  
> [WSL2中使用windows的代理](https://lvjianqiao.top/2020/11/22/WSL2%E4%B8%AD%E4%BD%BF%E7%94%A8windows%E7%9A%84%E4%BB%A3%E7%90%86/)  
> [记录一次WSL2的网络代理配置](https://jiayaoo3o.github.io/2020/06/23/%E8%AE%B0%E5%BD%95%E4%B8%80%E6%AC%A1WSL2%E7%9A%84%E7%BD%91%E7%BB%9C%E4%BB%A3%E7%90%86%E9%85%8D%E7%BD%AE/)