---
title: Android无线传输的另一种解决办法——WebDAV
date: 2013-01-12T01:46:00+08:00
categories: 手机
tags: [android,webdav]
---

本来一直使用强大的Samba Filesharing，但是在JB系统下却不好用，只能寻找别的办法，在Google Play里翻来翻去，翻出了WebDav server，试用了一下，非常不错，于是乎去广告+汉化+美化版诞生。

先上一个主界面的图：

![](/uploads/2013/01/webdav-server.png)<!--more-->

界面很简单，设置界面我也已经汉化，基本上不用动什么了。值得说明的是建议在WIFI使用静态IP，方便软件的连接。

![](/uploads/2013/01/wifi-settings.png)

先给出[下载链接](/uploads/2013/01/webdav-server-v1.7-chs-adfree.apk)。

如果你会使用WebDAV的话，请看到此为止，如果不行，那么请继续看如何使用Windows连接一个WebDAV服务器。

首先，右键点击“网上邻居”，选择“映射网络驱动器”。

![](/uploads/2013/01/webdav01.png)

弹出的界面选择“注册联机存储或连接到网络服务器”。

![](/uploads/2013/01/webdav02.png)

到这里一直继续~~

![](/uploads/2013/01/webdav03.png)

这里填上你在Android手机里看到的地址，我的就是图里那个，呵呵~~

![](/uploads/2013/01/webdav04.png)

下一步可以起个名字：

![](/uploads/2013/01/webdav05.png)

一劳永逸(这就是我前面说的最好用静态IP)，以后可以直接在网上邻居中打开你手机里的文件了，完美支持中文，不像FTP服务器那样对中文支持不好~~

![](/uploads/2013/01/webdav06.png)

另外可以使用命令行连接，请自行Google。
