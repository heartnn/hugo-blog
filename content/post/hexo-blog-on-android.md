---
title: 在Android手机搭建Hexo博客环境
date: 2016-12-24T11:36:44+08:00
categories: 技术
tags: [android,hexo]
---

![](/uploads/2016/12/hexo-logo.png)

### 方案对比

#### Termux

[Termux](https://play.google.com/store/apps/details?id=com.termux)是比较简单的Linux终端，提供简单的开发环境，用来Hexo发布很方便，缺点是终端不支持中文，作者也没有提供多语言。

#### Linux Deploy

[Linux Deploy](https://play.google.com/store/apps/details?id=ru.meefik.linuxdeploy)可以部署多种Linux发行版，比如Debian和Ubuntu等，可用ssh和图形访问，和PC上没有太多区别。缺点是占用空间可能会较大。<!--more-->

### 系统部署

#### Termux

```bash
apt update && apt upgrade  //升级到最新版本
apt install coreutils      //安装一些基本的工具
apt install git            //安装Git
apt install nodejs         //安装Node.js
```

#### Linux Deploy

这个要稍微复杂一些，安装完后可以在左上角的主菜单内调整字体和语言，点击右下角可以配置当前的profile，建议使用Debian或Ubuntu，因为别的不容易找到国内镜像(国内镜像一般以amd64为主，支持arm的很少)。

```plain
http://mirrors.ustc.edu.cn/debian/       //Debian中文源
http://mirrors.ustc.edu.cn/ubuntu-ports/ //Ubuntu中文源
```
软件将系统默认存入内置SD卡的linux.img中，建议ssh和vnc至少开启一种，方便远程管理。

当部署完成后可以用ssh登录系统，执行`sudo apt-get install git-core`安装Git。

Node.js安装需要利用nvm:

```bash
wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.32.1/install.sh | bash
```

详细的可以参考[nvm相关说明](https://github.com/creationix/nvm)。

### 环境配置

#### Git

```bash
git config --global user.email "xxxx@xx.com"  //邮箱
$ git config --global user.name "xxxx"        //用户名
$ ssh-keygen -t rsa -C "xxxx@xx.com"          //生成配对的公私钥
```

#### NPM

```bash
npm config set registry http://registry.npm.taobao.org
```

### 安装和使用Hexo

执行`npm install hexo-cli -g`安装Hexo，并使用Git存放博客相关文件。
