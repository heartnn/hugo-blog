---
title: "Android 5.1下SD卡格式化问题"
date: 2015-06-19T21:52:00+08:00
categories: 软件
tags: [android]
---

Android 5.1采用授权方式管理SD卡的写入操作，相对于之前的4.4方便多了，而heartnn也由于折腾，碰上了一点小问题，下面详细说下原因：

前阵子将手机的SD卡格式成了exFat(关于exFat的优点请自行Google)，由于平时用的比较多的是[ES文件管理器](http://coolapk.com/apk/com.estrongs.android.pop)，用着也没发现什么问题。最近发现用其它的文件管理器时，会发生SD卡授权时只显示内置存储，而且在使用浏览器上传文件时也无法看到外置卡。

值得注意的是这也许和Rom的支持有关系，在Xperia Z Ultra GPe版本的Rom中是存在这个问题的，其它Rom不好说，所以为了兼容性，SD卡还是格式化成Fat32，尽量不要exFat，除非需要复制超过4GB大小的文件。
