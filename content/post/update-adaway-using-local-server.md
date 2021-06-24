---
title: AdAway的一些使用技巧
date: 2015-01-29T23:31:00+08:00
categories: 手机
tags: [hosts,android]
---

![](/uploads/2015/01/adaway.png)

AdAway有点像电脑上的adblock，可以通过订阅源来下载更新。heartnn在手机上也经常用AdAway来合并几个源，并且还可以加上自己的黑名单和白名单，非常方便。

有时某些源不提供直接订阅地址了，有些是压缩包，都是无法直接导入的，这时还需要用Adaway的话，就需要设置一番了。

所有方法，包括heartnn的方法，都是万变不离其宗的，因为AdAway只支持http和https，所以只需要手机本地建立一个web服务器即可。<!--more-->

使用一个很简单的服务器软件，[SimpleHttpServer](https://play.google.com/store/apps/details?id=jp.ubi.common.http.server)，其他类似的也可以。

软件主界面是这样的，开启服务器后，就显示上面那一行网址(在没有网络的时候会什么都不显示)，此时不用管那么多，直接在浏览器里输入<http://127.0.0.1:12345>，网页会显示手机的sdcard目录，假设我们把其中一个hosts文件下载到了Download/hosts，那么你需要在AdAway里填写的订阅源就是<http://127.0.0.1:12345/Download/hosts>。

![](/uploads/2015/01/simplehttpserver.png)

此软件的设置部分也很简单，基本上需要修改的也就是目录了，另外那个Use MediaStore没搞明白是什么。

![](/uploads/2015/01/simplehttpserver-settings.png)

以此类推，可以添加多个文件作为订阅源，而且当你下载了新的版本，AdAway也可以检查到更新，这样以来，所有的源又可以合并在一起了。
