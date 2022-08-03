---
title: "Hugo搭建博客"
date: 2022-08-04T00:58:00+08:00
draft: false
tag:
- hugo
---

## 安装

以Ubuntu20.04为例


```
sudo apt-get install hugo
```

但是apt仓库版本一般不是最新的. 如果想安装最新版本, 可以从hugo的[GitHub发行仓库](https://github.com/gohugoio/hugo/releases)上获取deb包并安装. 例如:


```
# 进入用户目录, 随意.
cd ~
# 获取deb文件, 自行选择合适的版本.
wget https://github.com/gohugoio/hugo/releases/download/v0.92.2/hugo_0.92.2_Linux-64bit.deb
# 安装
sudo dpkg -i hugo*.deb
# 查看hugo版本
hugo version
```

## 配置

### 主题

#### 安装主题

```
git submodule add git@github.com:adityatelange/hugo-PaperMod.git themes/hugo-PaperMod
```
[*git submodule命令详解*](https://git-scm.com/book/en/v2/Git-Tools-Submodules)

#### 使用主题

在配置文件```config.yaml```中使用如下配置项
```
theme: hugo-PaperMod
```

> 注意:  
> 如果重新git clone项目, 通过git submodule安装的主题, 并不会第一时间安装. 需要执行以下两条命令以安装主题.
> ```
> git submodule init
> git submodule update
> ```

## 基本操作

生成新文章
```
hugo new post/<filename>.md
```

为了简化操作, 在```.bashrc```文件里设置h函数
```
h(){
    hugo new post/$1.md
}
```

## 部署到GitHub Page

通常情况下, 网站的源代码放在私有仓库里, 然后进行自动化构建和部署. 本文以hugo源码托管在Github仓库, 通过Github Action自动化部署到Github Pages为例. 基本流程为:

1. 创建两个仓库: 私有库, 存放网站源码; 目标库, Github Pages(xxx.github.io).
2. 创建Personal Access Token并配置在私有库的secret中.
3. 创建私有库的Github Action Workflow的yaml文件, 配置自动化流程.

### 生成Token

创建PAT的主要目的是给私有库访问目标库的权限，可以让私有库的actions推送构建好的代码到目标库中去。

```
# 在Github中
Profile Photo -> Settings -> Developer settings -> Personal access tokens -> Generate new token
```

记得要勾选下面两个scope: workflow and write:packages.

### 在私有库设置Token

```
# 在私有库中
Settings -> Secrets -> Actions -> New repository secret
```

粘贴刚才生成的PAT TOKENXXX 到value框中, 并命名这个secret为SECRETNAME.

> token的名字要与workflow配置文件里一致

### 创建并配置workflow文件


在项目根生成```.github/workflows/gh-pages.yml```文件. 此yaml文件包括如下几个部分:

- Checkout: 拉取源码
- Setup Hugo: 配置Hugo构建环境
- Run Hugo: 构建网站
- Deploy: 用#GitHub Pages Deploy Action将网站发布到目标库内容如下.

```
name: GitHub Pages

on:
  push:
    branches:
      - main  # Set a branch to deploy
  pull_request:

jobs:
  deploy:
    runs-on: ubuntu-20.04
    concurrency:
      group: ${{ github.workflow }}-${{ github.ref }}
    steps:
      - uses: actions/checkout@v3
        with:
          submodules: true  # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: '0.92.2'
          extended: true

      - name: Build
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        if: ${{ github.ref == 'refs/heads/main' }}
        with:
          external_repository: KEAE12315/blog
          personal_token: ${{ secrets.ACTION_ACCESS_TOKEN }} 
          publish_dir: ./public
          publish_branch: hugo
```

## 参考资料

- [Github Action readme](https://github.com/peaceiris/actions-gh-pages#%EF%B8%8F-first-deployment-with-github_token) 
- [Hugo使用Github Action自动部署博客到Github Pages](https://tomial.github.io/posts/hugo%E4%BD%BF%E7%94%A8github-action%E8%87%AA%E5%8A%A8%E9%83%A8%E7%BD%B2%E5%8D%9A%E5%AE%A2%E5%88%B0github-pages/)
- [从私有代码库自动部署Hugo站到GitHub Pages](https://finisky.github.io/deployhugofromprivaterepo/)