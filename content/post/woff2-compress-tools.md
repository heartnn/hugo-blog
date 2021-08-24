---
title: woff2转换工具
date: 2021-08-24T15:22:38+08:00
categories: 软件
tags: [woff2,ttf]
---

最近在研究使用Jellyfin，由于字幕的问题需要备用的woff2字体，网上的字体都不是很完整的，所以想自己转换一些使用。这个工具是从[Google的代码](https://github.com/google/woff2)编译而来，是用Cygwin编译的，Windows下可以使用。

[下载](/uploads/2021/08/woff2-compress-tools.7z)后打开，其中包括`woff2_compress.exe`和`woff2_decompress.exe`，使用方法很简单：

```
woff2_compress myfont.ttf
woff2_decompress myfont.woff2
```

转换成woff2时，输入文件可以是ttf和otf，但ttc是不行的，需要将ttc转换成ttf使用。
