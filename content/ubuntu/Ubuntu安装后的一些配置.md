---
title: "Ubuntu安装后配置"
date: 2022-08-05T03:19:27+08:00
draft: false
tags:
- Linux
- Ubuntu
- Configuration
---

### 查看系统版本

```
# 操作系统内核信息
uname －a

# 操作系统版本信息
cat /proc/version

# 操作系统发行版信息
cat /etc/issue

# cpu相关信息
cat /proc/cpuinfo

# 版本多少位
getconf LONG_BIT

lsb_release -a
```

## 设置Git

### 本地Git用户邮箱设置


```
# 查看git配置信息
git config --list

# 查看用户名
git config user.name

# 查看邮箱
git config user.email

# 设置全局用户名
git config --global user.name ""

# 设置全局邮箱
git config --global user.email ""
```

### 生成SSH key

```
ssh-keygen -t ed25519 -C ""
```

一路回车, 默认生成到```~/.ssh```目录下.

将后缀为```.pub```文件的内容添加到GitHub账户里.

## 换源

Ubuntu 的软件源配置文件是 ```/etc/apt/sources.list```

备份配置文件
```
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
```

修改文件
```
sudo vim /etc/apt/sources.list
```

清华源
```
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-security main restricted universe multiverse

# 预发布软件源，不建议启用
# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-proposed main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-proposed main restricted universe multiverse
```

更新软件列表：
```
sudo apt-get update  
```

更新软件
```
sudo apt-get upgrade
```

### 一些其它的源

阿里源
```
deb http://mirrors.aliyun.com/ubuntu/ jammy main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ jammy-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ jammy-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ jammy-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ jammy-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy-backports main restricted universe multiverse
```

中科大源
```
deb https://mirrors.ustc.edu.cn/ubuntu/ jammy main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ jammy main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ jammy-security main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ jammy-security main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ jammy-proposed main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ jammy-proposed main restricted universe multiverse
```

网易163源
```
deb http://mirrors.163.com/ubuntu/ jammy main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ jammy-security main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ jammy-updates main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ jammy-proposed main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ jammy-backports main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ jammy main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ jammy-security main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ jammy-updates main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ jammy-proposed main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ jammy-backports main restricted universe multiverse
```

## .bashrc配置

```
# apt-get
alias au="sudo apt-get update && sudo apt-get upgrade -y"
alias s="source ~/.bashrc"

# git
alias gs="git status"
gac(){
    git add .
    git commit -m "$1"
}
gaca(){
    git add .
    git commit -m "$1" --amend
    git push -f
}

# pipenv
export PIPENV_VENV_IN_PROJECT=1
alias pi="pipenv install"
alias va="pipenv shell"

# nodejs
alias nrs="npm run serve"

# hugo
alias hsd="rm -rf public/*; hugo serve -D"
h(){
    hugo new post/$1.md
}

# tabby
# Shell working directory reporting
export PS1="$PS1\[\e]1337;CurrentDir="'$(pwd)\a\]'
```

> 参考资料  
> [Ubuntu 镜像使用帮助](https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/)  
> [git全局设置用户名跟邮箱相关命令](https://segmentfault.com/a/1190000038802019)