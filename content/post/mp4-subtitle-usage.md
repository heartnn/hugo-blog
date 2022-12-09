---
title: 利用MP4Box解出tx3g格式字幕
date: 2022-12-09T16:08:20+08:00
categories: 软件
tags: [mp4box]
---

首先说明的是，[MKVToolNix](https://mkvtoolnix.download/)无法加载tx3g格式的字幕，这种字幕一般封装在mp4文件中，以网上下载的居多。另外用[BBDown](https://github.com/nilaoda/BBDown)下载的bilibili视频如果带字幕的话，也是这种格式。

以某集电视剧为例，文件名：`IRIS.S02E01.2013.WEB-DL.1080p.H265.AAC-Xiaomi.mp4`

首先需要了解字幕文件所在的轨道号：<!--more-->

    mp4box IRIS.S02E01.2013.WEB-DL.1080p.H265.AAC-Xiaomi.mp4 -info

```
# Movie Info - 4 tracks - TimeScale 1000
Duration 01:03:24.395
Fragmented: no
Progressive (moov before mdat)
Major Brand isom - version 512 - compatible brands: isom iso2 mp41
Created: UNKNOWN DATE

# Track 1 Info - ID 1 - TimeScale 90000
Media Duration 01:03:24.346  (recomputed 01:03:24.429)
Track has 1 edits: track duration is 01:03:24.346
Track flags: Enabled In Movie
Media Info: Language "Korean (kor)" - Type "vide:hev1" - 91213 samples
...

# Track 2 Info - ID 2 - TimeScale 48000
Media Duration 01:03:24.394
Track has 1 edits: track duration is 01:03:24.395
Track flags: Enabled In Movie
Media Info: Language "Korean (kor)" - Type "soun:mp4a" - 178331 samples
...

# Track 3 Info - ID 4 - TimeScale 1000000
Media Duration 01:03:11.699
Track has 1 edits: track duration is 01:03:11.699
Track flags: Enabled In Movie
Media Info: Language "chi (chi)" - Type "sbtl:tx3g" - 833 samples
...

# Track 4 Info - ID 5 - TimeScale 1000000
Media Duration 01:03:11.699
Track has 1 edits: track duration is 01:03:11.699
Track flags: Disabled In Movie
Media Info: Language "chi (chi)" - Type "sbtl:tx3g" - 833 samples
...
```

大致信息如上，其中ID4和ID5就是两个字母轨道。

分别以srt格式解出两个轨道：

```
mp4box IRIS.S02E01.2013.WEB-DL.1080p.H265.AAC-Xiaomi.mp4 -srt 4
mp4box IRIS.S02E01.2013.WEB-DL.1080p.H265.AAC-Xiaomi.mp4 -srt 5
```

成功的话，结果会显示：`Conversion done`，得到的字幕可以重新混流成mkv格式。

旧版的MP4Box只能支持srt格式，新版[MP4Box](https://github.com/gpac/gpac#mp4box)可以支持ssa、webvtt等格式。

有些时候需要重新对字母轨道重新排序，比如上面的文件，想要繁体中文在简体中文之间，需要的命令是：

    mp4box IRIS.S02E01.2013.WEB-DL.1080p.H265.AAC-Xiaomi.mp4 -swap-track-id 4:5

结果显示：`Saving IRIS.S02E01.2013.WEB-DL.1080p.H265.AAC-Xiaomi.mp4: In-place rewrite`。

参考文章：

- https://github.com/gpac/gpac/wiki/mp4box-gen-opts
- https://web-dl.cc/ffmpeg-mp4box-subtitleedit
