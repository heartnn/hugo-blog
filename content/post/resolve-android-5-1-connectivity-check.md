---
title: 解决Android 5.1的叹号问题
date: 2015-03-30T00:53:00+08:00
categories: 手机
tags: [android,hosts]
---

众所周知，Android 5.0以后引入了网络图标的那个叹号，在国内真的是很郁闷，因为普通方式根本无法连接<http://clients3.google.com/generate_204>，所以都借助于[小狐狸的工具](http://www.noisyfox.cn/45.html)，或者更暴力的：

```dos
adb shell "settings put global captive_portal_detection_enabled 0"
```

再或者给`clients3.google.com`添加一条hosts。

升级到5.1后，叹号又回来了，我没有试上面那些方式，而是依然使用hosts解决，这次Google更改了验证服务器，我鄙视他。。。<!--more-->

以上都是废话，下面是关键的域名：

```plain
connectivitycheck.android.com
```

给这个域名找一个能上Google的IP，这样叹号就消失了。
