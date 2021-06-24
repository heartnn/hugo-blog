---
title: "Acrylic DNS Proxy简易教程"
date: 2015-04-02T22:52:00+08:00
categories: 软件
tags: [proxy,dns]
---

最近更新了一份基于Acrylic DNS Proxy的hosts文档，在此做一个简易的说明。

首先在[官方网站下载](http://mayakron.altervista.org/)软件(打不开官网的话可以直接到[sf.net](http://sourceforge.net/projects/acrylic/)下载)，安装完成后，在开始菜单后可以找到下面几个快捷方式：

![](/uploads/2015/04/acrylic-console.png)

在此仅说明一下重要的配置文件，注意看`PrimaryServerAddress`的部分，默认会是`8.8.8.8`，然后对应`SecondaryServerAddress`是`8.8.4.4`，由于我们将来会使用hosts文档去解析一些网站，其实这里设置成国内的DNS就可以了，比如114DNS，至于端口保存53就行。

下面`HitLogFileName`可以自己设置一下，有时会在log里发现一些新的域名，从而知道某些网站打不开的原因。

这里`HitLogFileWhat=BHCFRU`对应的是上面的说明，建议可以只关注自己需要的。<!--more-->

- B -> Explicitly blocked
- H -> Resolved from the HOSTS cache
- C -> Resolved from the Acrylic cache
- F -> Forwarded to the configured DNS servers
- R -> Received from one of the configured DNS servers
- U -> Silent update from one of the configured DNS servers

最后两点：

1. 打开`Edit Acrylic Hosts File`，将网上找到的hosts文档内容复制进去。
2. 将本地网络连接IPv4的主DNS改为`127.0.0.1`，副DNS空着就可以了。

需要注意的是每次修改了hosts后，需要执行`Purge Acrylic Cache Data`才可以生效。

以上是对Acrylic DNS Proxy的简易使用方法，更深层次的使用参见配置文件里的说明。
