---
title: 在Fedora中使用金山快盘
date: 2015-03-29T08:53:00+08:00
categories: 技术
tags: [fedora]
---

最近百度云和360云盘都有限速的迹象，反倒是金山快盘没有限速，遂将个人文档迁移到金山中，正好金山提供各种平台的客户端，算是国内最全面的了，可是Linux客户端还是只提供了.deb的安装包，也不知道源代码在哪里。。。这么霸气的节奏，都不开源的吗？还是我没找到。

网络中提供了在Fedora中安装金山快盘客户端的方法，本人用的Fedora 21 Mate，仅在此环境下说明。

在[麒麟官网下载](http://www.ubuntukylin.com/applications/showimg.php?lang=cn&id=21)deb安装包并解压缩。

进入解压缩后的文件夹，复制usr,opt文件夹到文件系统中<!--more-->

```bash
sudo cp -r usr /
sudo cp -r opt /
```

然后安装依赖的库文件(可能不需要，如果不确定，请先暂时往后看)

```bash
sudo yum install libqxt
sudo yum install boost-iostreams
```

此时可以尝试在终端启动`/usr/bin/kuaipan4uk`，如果是Fedora 21的话，可能会提示找不到libboost-iostreams-1.54.0。(因为deb包对应的是Ubuntu 14.04，而Fedora 21中相应的包要更新一些，所以必须从网上下载1.54.0的版本，对应的是Fedora 20中的rpm包)

从各大源中搜索boost-iostreams-1.54.0-12.fc20.x86_64.rpm(32位系统请搜索boost-iostreams-1.54.0-12.fc20.i686.rpm)，然后将`usr/lib64`中的内容复制到文件系统的相应位置(32位系统对应`/usr/lib`)。

至此，应该可以正常启动了，并且在系统菜单中也有相应的项目，如果没有的话，可以对`/usr/bin/kuaipan4uk`创建链接。
