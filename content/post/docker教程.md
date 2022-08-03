---
title: "Docker教程"
date: 2022-08-08T00:29:27+08:00
draft: false
tags:
- Docker
- Configuration
---

本文默认环境为Linux，打包镜像以Python为例。

## 常用操作

查看镜像：docker images
查看所有容器：docker ps -a
查看所有容器ID：docker ps -aq
查看所有正在运行容器：docker ps
停止容器：docker stop <containerID>
停止所有容器：docker stop $(docker ps -aq)
删除容器：docker rm <container ID>
删除镜像：docker rmi <image ID>
删除所有停止的容器：docker container prune -f
删除所有不使用的镜像：docker image prune -f -a
保存镜像：docker save <imageID> > <filename>
加载镜像：docker load < <filename>

*注意，删除前需要先停止容器。*

## 安装

官方安装教程
- [Ubuntu](https://docs.docker.com/engine/install/ubuntu/)
- [Windows](https://docs.docker.com/docker-for-windows/install/)
- [MacOS](https://docs.docker.com/docker-for-windows/install/)

## 配置

[官方原文](https://docs.docker.com/engine/install/linux-postinstall/)过长，摘取了常用的两个步骤。

### 配置root用户

Docker绑定到Unix socket，但它需要sudo权限。如果不想每次都加sudo命令，创建一个名为Docker的Unix组.

```
# 创建docker组。
sudo groupadd docker

# 将用户添加到docker组。
sudo usermod -aG docker $USER

# 激活对组的更改
newgrp docker 
```

### 配置开机启动

注意，在 Debian 和 Ubuntu 上，Docker 服务被配置为默认启动。要在其他发行版的引导中自动启动 Docker 和 Containerd，请使用以下命令:

```
# 设置自动启动
sudo systemctl enable docker.service
sudo systemctl enable containerd.service

# 若要禁用此行为，请使用 disable。
sudo systemctl disable docker.service
sudo systemctl disable containerd.service

# 手动开关docker服务
sudo service docker start
systemctl stop docker
```

## Build your Python image

1. 生成requirements.txt

    ```
    pip3 freeze > requirements.txt
    ```
2. 创建Dockerfile和.dockerignore（可选）

    ```
    # syntax=docker/dockerfile:1

    FROM python:3.8-slim-buster

    WORKDIR /app

    COPY requirements.txt requirements.txt
    RUN pip3 install -r requirements.txt

    COPY . .

    CMD [ "python3", "-m" , "flask", "run", "--host=0.0.0.0"]

    ```
3. 生成镜像

    ```
    docker build -t <tag name> .
    ```
4. 运行镜像

    ```
    docker run -it -v <filepath>:<docker path> -p 127.0.0.1:5001:5000 <image name>
    ```
5. 保存镜像

    ```
    docker save -o <path> <image name>
    ```


