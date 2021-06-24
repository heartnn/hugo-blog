---
title: AAC封装M4A简易工具
date: 2016-11-12T08:01:58+08:00
categories: 软件
tags: [mp4box]
---

最近手头一大堆的 mp4 视频，从中提取了音频部分，准备放在手机上听，而 mp4 一般提取音频是 aac 格式，这种文件放在 foobar2000 里播放的时候是没有进度条的，所以必须封装为 m4a，m4a 本身其实就是 mp4。

工具最简单的用 mp4box 就可以了，也可以用 ffmpeg，不过这货过于强大，而且兼容性不如 mp4box。

原理参考[这里]({{< relref "mp4-package.md" >}}):

```dos
mp4box.exe -add 1.aac:name="" -new 1.m4a
```

顺便做了个[批处理工具](https://www.lanzous.com/i83vmqf)，直接把要转换的 aac 拖入即可。
