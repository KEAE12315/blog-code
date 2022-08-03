---
title: "VS Code远程连接"
date: 2022-08-08T00:39:12+08:00
draft: false
tags:
- VS Code
- Configuration
---

微软针对VS Code的远程开发推出了三个插件，包括: 
- Remote-SSH: SSH 连接虚拟/实体Linux主机;
- Remote-Container: 连接容器;
- Remote-WSL: 连接WSL(也就是Linux子系统). 重点服务使用Windows但具有Linux开发需求的用户. 

## 使用Remote-SSH连接实验室服务器

这里是微软关于SSH连接的[官方文档](https://code.visualstudio.com/docs/remote/ssh#_getting-started)

### 安装Remote-SSH插件

在Extension侧边栏搜索Remote-ssh并安装

### 修改ssh配置文件

在VS Code里crtl+shift+p，打开命令行输入open ssh, 选择第一个选项```Remote-SSH: Open SSH Configuration File...```, 可直接打开```C:\Users\<user>\.ssh\config```文件. 也可以直接编辑该文件. 如果没有这个文件夹，在管理可选功能里添加 OpenSSH 客户端. 


在config文件中写入以下内容

```
Host Labserve #填写别名
  HostName 1.1.1.1 #填写实验室服务器IP/host
  User user #填写ssh用户名
  Port port #填写端口号. 可缺省该配置, 默认为22.
```

### 添加ssh公钥

远程ssh连接Linux服务器时，需要借助ssh公钥登录.

1. 客户端生成ssh公钥  

在cmd中输入```ssh-keygen```，一路回车键，完成后在```$HOME/.ssh/```目录下可以发现两个文件: id_rsa.pub和id_rsa，分别是客户端的公钥和私钥. 

2. 上传ssh公钥  
登录远程服务器，创建.ssh目录和authorized_keys文件

    ```
    mkdir ~/.ssh/
    cd ~/.ssh/
    vim authorized_keys
    ```

3. 修改服务器端ssh配置

    ```
    vim /etc/ssh/sshd_config
    ```
    ```
    RSAAuthentication yes
    PubkeyAuthentication yes
    AuthorizedKeysFile $h/.ssh/authorized_keys
    ```

保存并退出vim. 

4. 重启ssh服务
    ```
    service ssh restart
    ```

### 通过CONNECTION侧边栏进行连接

此时，已经可以在CONNECTION侧边栏看见服务器的别名，点击进行连接. 
特别的，如果没有生成ssh公钥，这一步可能会要求输入服务器密码. 

## 问题汇总

### 阿里云、腾讯云等云服务器连接超时

目前VSCode的远程连接插件，默认勾选Use Local Server，也就是默认使用本地局域网络进行连接. 如果你遇到阿里云等云服务器连接超时，应该取消勾选此项. 

### 用wget不能解析xxx或者任意远程服务下载失败的

vscode最近新增了一个超级实用的配置: Allow Local Server Download:

如果在远程主机下载vscode远程服务失败，改在本地电脑下载此服务并且用scp进行传输. 
所以，开启这个选项理论上可以解决之前的任意下载问题. 

### 端口转发

[VS Code官方文档](https://code.visualstudio.com/docs/remote/ssh)有解决办法: 直接在配置文件那里写上. 


> 参考资料  
> [VSCode Remote 体验 | 远程Linux环境开发真香](https://zhuanlan.zhihu.com/p/64849549)  
> [VS Code Remote配置](https://zhuanlan.zhihu.com/p/124105812)