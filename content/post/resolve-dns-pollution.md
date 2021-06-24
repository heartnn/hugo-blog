---
title: 关于DNS污染的解决办法
date: 2014-12-30T23:53:00+08:00
categories: 网络
tags: [chrome,dns]
---

第一种方法：切换公共DNS服务器就算是第一种方法。

关于公共DNS服务器列表请参考[这个帖子]({{< relref "public-dns.md" >}})，其中Google的应该是最经常用到的，用这个DNS也能解决某些Google服务的问题。

第二种方法：DNS加密，具体方法请自行Google。

重点说下第三种方法，如果你在用Chrome，那么除了hosts文件外，还应该做以下设置：

打开`chrome://flags`，找到下面的项目：

![](/uploads/2014/12/chrome-flags.png)<!--more-->

这里我没有开启实验性QUIC协议，据某些网友说可能会降低浏览速度。但是内置异步DNS的效果很好，因为本地解析到`*.l.google.com`或类似域名的时候，可能会忽略hosts文件中的IP，从而解析到被屏蔽的IP地址。(今天自己发现访问谷歌Chrome商店时无法打开，经过开启内置异步DNS后没问题了。)

以上需配合hosts实现真正的无障碍访问Google服务，单凭hosts或者单凭DNS都不能完全解决问题。
