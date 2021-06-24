---
title: 关于MP4封装的一些事
date: 2015-07-28T00:17:33+08:00
lastmod: 2016-04-01T07:16:16+08:00
categories: 软件
tags: [mp4box]
---

众所周知，Youtube提供的视频，其中最高规格是1080p，但却只有video only，如果想下载到本地的话，是没有声音的，有声版本最高是720p.h264.aac，这时，便需要提取这个720p的音频部分，再加上1080p的视频部分进行封装。

封装的工具大多是ffmpeg或者[mp4box](http://www.videohelp.com/software/mp4box)，其中包含有很多前端，比如My MP4Box GUI或者YAMB。

最近，heartnn在potplayer播放某个自己已经封装完的视频后，发现下面的问题：

![](/uploads/2015/07/my-mp4box-gui.png)<!--more-->

这个文件标明了用My MP4Box GUI生成。。。这对于强迫症怎么受的了。而且下面的Video和Audio部分title分别是封装前的文件名，对比下载的Youtube视频，根本没有这些信息的，这些在我看来只能算推广广告。

于是直接下载mp4box，用命令行来进行封装，以下假设需要封装的视频(1.h264)和音频(2.aac)文件和mp4box.exe位于同一目录下，封装后的文件名为file.mp4。

由于我们要去掉title，所以命令行要这样写：

```dos
mp4box -add 1.h264:fps=23.976:name="" -add 2.aac:name="" -new file.mp4
```

这里需要注意的是视频部分一定要指定fps，否则会按25.000处理，到时候音画不同步了可别怪我没提醒。

完成后可以利用`mp4box -info file.mp4`查看文件信息，而且放到potplayer里也不会看到那些没用的信息了。

另外如果需要解开封装的话，似乎必须一个一个track来解开(这部分我看完帮助没怎么明白，难道不能一次都解出来吗)。

```dos
mp4box -raw 1 file.mp4
mp4box -raw 2 file.mp4
```

以上都是基础用法，更多复杂的命令，heartnn暂时也用不到，如果有需要可以用`mp4box -h`来查看。

最后附上heartnn从安装包中提取的mp4box (下载链接: https://pan.baidu.com/s/1eRFTe9c 密码: nr6d)

### 参考文章

- https://sulisu.wordpress.com/2011/04/16/土豆网下载的f4v无损转aac无损转换m4a格式/
- http://blog.sina.com.cn/s/blog_5062b3790100ie55.html
