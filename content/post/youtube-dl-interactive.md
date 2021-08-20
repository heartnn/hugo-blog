---
title: 简单的youtube-dl交互下载
date: 2021-08-19T09:12:20+08:00
categories: 日常
tags: [youtube,windows]
---

[youtube-dl](https://github.com/ytdl-org/youtube-dl/)本身是个强大的工具，但是命令记起来还是有些繁琐的，而GUI工具也没发现特别好用的，所以有了下面的脚本。

### 使用方法

1. 下载最新版本的[youtube-dl.exe](https://github.com/ytdl-org/youtube-dl/releases)；
2. 下载[ffmpeg](https://www.ffmpeg.org/download.html#build-windows)，版本需大于3.4.2，否则无法合并webm；
3. 下载最新的[aria2c.exe](https://github.com/aria2/aria2/releases)，这样youtube-dl可以调用它实现多线程下载；
4. 上面三个文件放到脚本目录下的libs目录里；
5. 自行修改脚本中`--proxy`字段；
6. 下载得到的视频文件在脚本目录里。<!--more-->

### 关于脚本的一些简单说明

1、关于视频编码

- avc1：也就是h264的格式，一般现在经常使用的格式，许多up主也是以这种格式上传的
- webm：内封的是vp9格式，属于Google为了避免h265的高额费用开发的自有格式，在大部分时候是比avc1要小一些的。
- av01：比较新的格式，后缀也是mp4，但目前阶段基本没法硬解，同等清晰度下生成的文件比较小。
- best：下载youtube-dl自认为最好的版本（然而没必要）。

2、关于音频编码

在利于封装的原则下，avc1和av01首选m4a，最后生成的是mp4文件，webm对应opus音频。需要注意的是Youtube在处理m4a音频时，16kHz以上有“剃头”现象。

### 脚本下载及更新记录

[下载链接](/uploads/2021/08/youtube-dl-interactive.rar)

- 20210820：更新判断视频FPS，优先下载60fps的视频进行混流。
