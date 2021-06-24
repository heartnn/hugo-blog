---
title: WebP与博客封面
date: 2015-08-13T23:42:00+08:00
image: "/uploads/2015/08/webp-logo.png"
categories: 日常
tags: [blog,webp]
---

[WebP](https://developers.google.com/speed/webp/)是一种同时提供了有损压缩与无损压缩的图片文件格式，派生自视频编码格式VP8，是由Google在购买On2 Technologies后发展出来，以BSD授权条款发布。*via [维基百科](https://zh.wikipedia.org/wiki/WebP)*

超高的压缩比使得WebP可以在保持品质的同时，减少图片的大小。WebP可以在Google网站上下载专用的[命令行程序](https://developers.google.com/speed/webp/docs/precompiled)，也可以利用网络上许多集成了WebP的应用，比如Image Resizer等等。<!--more-->

目前看来，只有Chrome或者基于Chromium的浏览器可以正常浏览WebP的图片，似乎IE和Firefox都不能正常显示，只能显示灰黑色的placeholder。

在网络上可以下载到WebP格式图片的网站也是比较少的，[Upsplash](https://unsplash.com/)提供了多种图片格式下载，但需要一点小技巧，比如[这个图片](https://unsplash.com/photos/cGe1PV_yEso)，它的下载地址是类似于这样的：

> https://images.unsplash.com/photo-1428550670225-15f007f6f1ba?q=80&fm=jpg&s=bb2ea817134da9d8b90f305b07d2226e

我们可以将其中的jpg替换成png，从而下载到高清的原始图片，也可以替换成webp用来下载一个较小的文件用于网络。

对于Ghost这样的博客平台，网络上甚至有高人提供了程序，可以用来随机显示封面，网址是这样的：<https://unsplash.it/2500/1600/?random>，每次打开网址都可以显示不同的封面，作者提供的[源代码在此](https://github.com/DMarby/unsplash-it)。
