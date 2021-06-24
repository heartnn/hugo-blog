---
title: FLV合并的批处理办法
date: 2019-02-12T11:15:11+08:00
categories: 软件
tags: [ffmpeg]
---

过年的时候冲了B站大会员，在使用[Bilibili Evolved](https://github.com/the1812/Bilibili-Evolved)下载番剧时发现，有时下载来的是个zip文件，里面是分段的flv，虽然手头有MKVToolNix可以合并，但flv转mkv再转mp4实在是麻烦，又不想下载其它的工具，心想是不是可以使用ffmpeg的命令行搞定，于是有了下面的代码：

```bat
(for %%i in (*.flv) do @echo file '%%i') > list.txt
ffmpeg.exe -f concat -safe 0 -i list.txt -c copy output.flv
ffmpeg.exe -i output.flv -vcodec copy -acodec copy output.mp4
pause
del *.flv
del list.txt
```

使用方法：<!--more-->

1. 先下载[ffmpeg.exe](https://www.ffmpeg.org/download.html#build-windows)；
2. 把上面的代码存为批处理文件，和ffmepg.exe放在同一文件夹下；
3. 把分段的flv也放在这个文件夹下；
4. 运行bat，会自动生成mp4(无损转换)。

PS. 需要注意的地方是如果视频分段达到10以上时，Windows会按照1、10、2、3、4...的顺序，请注意排序（Windows下没有`sort -n`也是郁闷）。
