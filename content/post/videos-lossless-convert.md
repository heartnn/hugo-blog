---
title: 各类视频格式的无损转换
date: 2016-01-10T01:15:57+08:00
catefories: 软件
tags: [mkv,ffmpeg]
---

之前记录了[关于Mp4box的一些用法]({{< relref "mp4-package.md" >}})，但是mp4格式支持封装的音频和视频格式相对有限，另外还有外挂字幕等等，所以mp4并不是最好的选择。对于高清电影来说，网络上更多的是mkv。

mkv的工具主要是用[MKVToolNix](http://www.fosshub.com/MKVToolNix.html)和[gMKVExtractGUI](http://sourceforge.net/projects/gmkvextractgui/)(用来分离轨道)，都是GUI界面，像mp4之类的可以直接拖放到软件中进行处理，非常方便(mov格式也开始转向mpeg4，所以较新的mov格式也是可以直接编辑的)。

其它mkvtoolnix不支持的格式，可以利用[ffmpeg](http://ffmpeg.zeranoe.com/builds/)先转换为mkv，然后再交给mkvtoolnix处理：<!--more-->

```dos
ffmpeg -i input.wmv -map 0 -codec copy output.mkv
```

- -codec copy : 复制tracks至输出文件
- -map 0 : 强制选取输入文件#0内所有的tracks

**参考资料**

- [MKVToolNix - MKV 工具](http://www.5i01.cn/topicdetail.php?f=510&t=4192044)  
- [FFmpeg - 泛用影音转换工具](http://www.5i01.cn/topicdetail.php?f=510&t=3734550)

