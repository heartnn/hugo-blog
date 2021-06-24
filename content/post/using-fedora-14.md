---
title: "Fedora 14初级折腾"
date: 2010-11-14T06:30:00+08:00
categories: 技术
tags: [fedora]
---

![](/uploads/2010/11/fedora-14-release.png)

其实Fedora 14出来有一段时间了，上周用VMWare装了一次，因为选错了操作系统类型，所以安装到最后死机了。。。当时好像是选的Red Hat Enterprise，这次是选的Other Linux 2.6.x 64bit，而且把虚拟机的内存增加到了1024MB，这次终于是成功安装了。

![](/uploads/2010/11/fedora-settings.png)<!--more-->

![](/uploads/2010/11/fedora-memory.png)

说实在的，KDE的界面就是稍微多占了点内存，反应也貌似不如gnome快(个人认为)。

由于最近经常用VPN，所以虚拟机采用了桥接的方式，但是Fedora桥接的时候有了点困难，不像Ubuntu那么顺利，因为安装后Fedora没有对任意一个网卡进行配置，所以profile的地方时空的，还需要自己手工建立一个。点应用程序-->System Settings-->网络配置。

![](/uploads/2010/11/fedora-network-settings.png)

另外dns也要稍微设置一下，否则会连不上很多网，把第一个dns设置成宿主机的ip地址就行了。

换源是必须的，而且也很简单，不会的看这里: <http://mirrors.163.com/.help/fedora.html>
