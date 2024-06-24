---
title: mp4文件加入字幕显示
date: 2024-06-24T16:08:20+08:00
categories: 软件
tags: [mp4box]
---

众所周知，[MKVToolNix](https://mkvtoolnix.download/)可以为mkv文件加入多种字幕，但到了mp4，内嵌字幕会变的比较麻烦。起因是从Youtube下载了一部MV，由于音视频和字幕是分开下载的，而且下载的是AV1编码，就想着不要重新打包mkv，而是直接在mp4中嵌入字幕。

首先说原理，PotPlayer是可以识别到Codec ID为`tx3g`标签的字幕，`tx3g`是用于3GPP/MPEG时期的带有时间标记的文本，我们要利用这个特性，让字幕显示为`tx3g`，这样一般的播放器就都可以识别了。实现起来比较简单的方式就是使srt或者ttxt([GPAC Timed Text XML](https://wiki.gpac.io/xmlformats/TTXT-Format-Documentation/))会通过MP4Box直接显示为`tx3g`。

先将下载的字幕（我下载到的是webvtt格式）转换为srt，webvtt转换为srt比较简单，没有工具的情况下，作为文本文件打开，把标记WEBVTT文件头删掉就可以了。

然后通过下面的代码直接mux即可：<!--more-->

```
mp4box.exe -add "input.mp4#trackID=1:delay=0:lang=ja" ^
-add "input.mp4#trackID=2:delay=0:lang=ja" ^
-add "ja.srt#delay=0:hdlr="sbtl:tx3g":group=2:lang=ja:name=日文" ^
-add "en.srt#delay=0:hdlr="sbtl:tx3g":group=2:lang=en:name=英文" ^
-add "chs.srt#delay=0:hdlr="sbtl:tx3g":group=2:lang=zh:name=简体中文" ^
-add "cht.srt#delay=0:hdlr="sbtl:tx3g":group=2:lang=zh:name=繁體中文" ^
-for-test -new "output.mp4"
```

*不要在意这个`^`，因为粘贴到命令行窗口的时候会将这几行自动合并的。*

大致内容如上，需要调整的有字幕的数量、以及各个语言参数等等。

另外直接嵌入webvtt的参数是下面这样的，但PotPlayer不支持这种`wvtt`编码。

```
mp4box.exe -add "1.mp4#trackID=1:delay=0:lang=ja" ^
-add "1.mp4#trackID=2:delay=0:lang=ja" ^
-add "ja.vtt#delay=0:hdlr="text:wvtt":group=2:lang=ja:name=日文" ^
-add "en.vtt#delay=0:hdlr="text:wvtt":group=2:lang=en:name=英文" ^
-add "chs.vtt#delay=0:hdlr="text:wvtt":group=2:lang=zh:name=简体中文" ^
-add "cht.vtt#delay=0:hdlr="text:wvtt":group=2:lang=zh:name=繁體中文" ^
-for-test -new "output.mp4"
```

成功的话，放到PotPlayer里查看文件属性，然后`Text`栏下面会包含以下信息：

```
Format : Timed Text
Muxing mode : sbtl
Codec ID : tx3g
```

### 参考文章

- [GPAC wiki - DASH & HLS - Subtitles - Introduction](https://wiki.gpac.io/Howtos/subtitles/Subtitling-with-GPAC/)
