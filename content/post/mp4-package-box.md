---
title: MP4封装工具箱发布
date: 2019-04-04T10:36:12+08:00
categories: 软件
tags: [ffmpeg]
---

之前写过一个[aac转m4a的工具]({{< relref "aac-to-m4a.md" >}})，里面用到的程序是mp4box，这次就干脆把常用的功能整合一下，重新搞了一个，用的是ffmpeg了。

简单说一下几个批处理文件的功能。

1. aac封装m4a.bat：就是重新写的ffmpeg版本，现在版本迭代很快，兼容性应该没什么问题了。
2. flv转mp4.bat：这个就是网上流传的一键封装为mp4的批处理，加上了blv格式(为哔哩哔哩手机缓存，其实改后缀就是flv)。
3. mp4抽取音频.bat：有些时候只需要听音频的时候用，提取为m4a格式。
4. YouTube音视频合并(mp4+m4a).bat：YouTube现在的1080p以上视频和音频是分开的，这个批处理的作用就是将它们合并为mp4。

以上前3位需要拖放对应文件到批处理上，最后一个双击运行，按提示操作。

尤其是YouTube，以前都是用MKVToolNix先合并成mkv，然后再用Total Video Converter选视频和音频编码copy，才变成的mp4，为什么以前没直接搞成批处理。。。<!--more-->

最后是下载地址：<https://www.lanzous.com/i3o3u2h#bh6d>
