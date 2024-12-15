---
title: 升级群晖到7.21
date: 2024-12-15T09:49:42+08:00
categories: 技术
tags: [synology]
---

一直偷懒不愿意拆机拿出启动盘，所以竟一直没有更新，还停留在7.1版本，看到7.22删除了一些硬解相关的东西，更加确信没什么大问题的话就可以停留在7.21版本了。

启动盘用的[Redpill Recovery](https://github.com/RROrg/rr)，由于我的主板是个Intel N5095，所以启动盘制作直接选SA6400就好了，也不用设定`SataPortMap`之类的参数了（主要是我没有用Raid卡），编译前设定好半洗白，一路编译下去就搞定了。

然后更新完系统以后需要给Advanced Media Extensions打补丁，网上教程很多。

最后在Synology Photos里重新索引一下，系统就会自动转码HEIC图片了，非常开心。
