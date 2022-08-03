---
title: "使用frp进行内网穿透"
date: 2022-11-03T11:09:41+08:00
draft: false
categories:
- 
tags:
- Configuration
- frp
searchHidden: false
---

官方链接：https://github.com/fatedier/frp/releases  
请按照需要选择合适的版本, 可以直接下载也可以拷贝链接用命令行.

## 安装

以0.45版本, linux64位为例.

1. 使用wget下载: 
```
wget https://github.com/fatedier/frp/releases/download/v0.45.0/frp_0.45.0_linux_amd64.tar.gz
```
2. 解压frp压缩包
```
tar -zxvf frp_0.45.0_linux_amd64.tar.gz
```
3. 移动至/usr/local
```
mkdir /usr/local/frp
mv frp_0.45.0_linux_amd64/* /usr/local/frp/
./frpc -c ./frpc.ini
```

> 文件说明  
> 1. frps.ini: 服务端配置文件 
> 2. frps: 服务端软件 
> 3. frpc.ini: 客户端配置文件 
> 4. frpc: 客户端软件

## 配置systemctl控制

1. vim新建文件并写入配置内容

```
vim /usr/lib/systemd/system/frp.service
```
2. 写入以下内容，注意上文移动放置的路径和此处有关。这里是启动的服务端。
```
[Unit]
Description=The nginx HTTP and reverse proxy server
After=network.target remote-fs.target nss-lookup.target

[Service]
Type=simple
ExecStart=/usr/local/frp/frps -c /usr/local/frp/frps.ini
KillSignal=SIGQUIT
TimeoutStopSec=5
KillMode=process
PrivateTmp=true
StandardOutput=syslog
StandardError=inherit

[Install]
WantedBy=multi-user.target
```
3. 重新加载服务的配置文件
```
systemctl daemon-reload
```

现在就可以用 systemctl套装来控制frp了
```
# 启动
systemctl start frp
# 停止
systemctl stop frp
# 重启
systemctl restart frp
# 状态查看
systemctl status frp
# 设置开机自启
systemctl enable frp
# 关闭开机自启
systemctl disable frp

sudo journalctl -u frp
sudo vim /usr/local/frp/frps.ini
/usr/local/frp/frps -c /usr/local/frp/frps.ini
```

## 配置和使用

## 服务端

```
[common]
bind_port = 7000
token = 123
dashboard_port = 7500
dashboard_user = admin
dashboard_pwd = damin

[ssh]
listen_port = 6000
```

## 启动
