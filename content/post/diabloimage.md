---
title: "GAE相册程序推荐: diabloimage"
date: 2010-10-27T03:53:00+08:00
categories: 网络
tags: [python]
---

说起大菠萝相册来，应该是很久以前的东西了，可是鄙人是刚刚知道的（也许以前不太关心这个。），部署了一个，发现真的不错，于是推荐下。

![](/uploads/2010/10/diabloimage.jpg)<!--more-->

官方地址：<http://sa3.org/program/gae-album/>

源码下载：<http://code.google.com/p/diabloimage/>

这个程序严格说来是没有后台管理的，只有一个google账号的登录地址，还有上传地址，简单的不能再简单了，一开始heartnn还没有找到删除照片的地方，傻的跑到GAE的后台数据库里去了，最后发现打开一个图片的时候，在旁边的简介里有删除的链接，真是囧啊。。。

程序特点：随意外链、流量无限制(10.00 Gbytes/d)、绑定自己域名、图片不压缩，无水印。作为个人用图床足够了，只是缩略图的生成有点不爽，图图都是变形的了。。。

另发现0.07的版本配合bk.py总是出错，于是check svn得到了r11的版本，配合自己修改的bk.py终于可以备份了～～压缩包内备份说明的第一步就不用做了，记得上传的时候更改appid啊。

下载链接: https://pan.baidu.com/s/1dEX6RBj 密码: hn58

2010-11-17: 另作者已经释出新版[Sa3album](http://sa3.org/program/gae-album/)，是一个完全不一样的新程序了。
