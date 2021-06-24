---
title: 使用Shadowsocks为git代理
date: 2017-03-05T14:17:30+08:00
categories: 网络
tags: [git,shadowsocks]
---

一直在用官方的git程序更新位于Openshift的博客，今天更新时发现22端口非常缓慢，眼看博客代码提交不上去，所以只得挂上SS，但SS却不能为git做代理，于是才有了本文下面的内容。

#### 一、网上搜出了git为项目设置代理的方法：

```bash
git config --local http.proxy 'socks5://127.0.0.1:1080'
git config --local https.proxy 'socks5://127.0.0.1:1080'
```

此方法在我的电脑里并不适用，不过多讨论。<!--more-->

#### 二、使用[Proxifier](https://www.proxifier.com/)

这个据说可以全局代理，但是我也是失败了。仅送上注册码：

>L6Z8A-XY2J4-BTZ3P-ZZ7DF-A2Q9C (Portable Edition)  
5EZ8G-C3WL5-B56YG-SCXM9-6QZAP (Standard Edition)  
P427L-9Y552-5433E-8DSR3-58Z68 (MAC)

#### 三、使用[SocksCap64](https://www.sockscap64.com/)

这次是成功了的，需要把`C:\Program Files\Git\git-cmd.exe`添加进去，如果你用的是bash，就把`git-bash.exe`加进去(这里我没有测试)。

关于Linux应该有简洁的方法，以后更新。
