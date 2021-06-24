---
title: 用Fiddler查找访问错误的域名
date: 2015-01-16T23:55:00+08:00
lastmod: 2015-01-22T11:27:23+08:00
categories: 软件
tags: [proxy,fiddler]
---

有时朋友们总问我，为什么无法访问Google Now，因为自己不用Google Now,相关域名也不知道，所以我也没什么好的办法。

Fiddler这个工具很强大，它能告诉我们网页无法访问或者不能完全显示的元凶。

首先去它的[官方网站下载](http://www.telerik.com/download/fiddler)，我用的.NET 2的版本，应该都是一样的。

安装后打开，此时本地连接的DNS服务器会变成127.0.0.1，所有的流量会通过Fiddler，此时从电脑上访问的记录便会显示在Fiddler里：

![](/uploads/2015/01/fiddler.png)<!--more-->

上图中红色的部分便是造成访问障碍的域名了。

打开菜单`Tools-->Fiddler Options`，勾选`Decrypt HTTPS traffic`，然后勾选`Ignore server certificate errors`，如下图。此步骤会提示安装证书，安装就可以了，这样查看证书时会显示是Fiddler的，没有任何证书错误。

![](/uploads/2015/01/fiddler-ignore-server-certificate-errors.png)

然后如果想调试其他设备的话，需要允许远程连接：

![](/uploads/2015/01/fiddler-allow-remote-computers-to-connect.png)

然后将手机和电脑连入同一局域网内，设置手机Wifi的代理为手动：

![](/uploads/2015/01/android-wifi-proxy-option.png)

这样，手机中所有访问的域名都会记录到Fiddler中了。

关于更深入的使用，可以自行Google，此工具甚是强大，上面的说明也只是用到了九牛一毛而已。

本文最后更新时间: 2015-01-22 11:27:23
