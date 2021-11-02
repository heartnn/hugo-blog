---
title: "Flac转Apple Lossless的便捷方法"
date: 2015-05-19T03:46:00+08:00
lastmod: 2017-09-16
image: "/uploads/2015/05/foobar2000.png"
categories: 软件
tags: [flac,alac,foobar2000]
---

折腾这个的缘由是最近入了一个改版ipc，由于苹果这货只能支持mp3、aac和自家的无损，遂导入无损音乐时需要进行转换成Apple Lossless。

网上的方法大概有两种，一个就是利用cue挂载为虚拟光盘，然后用iTunes直接转换，二是利用编码转换工具进行转码。

第一种方法不多说了，几年前就已经广泛利用，其中用到的WinMount更是长时间都没更新过了。<!--more-->

第二种方法需要下载qaac、reflac或者ffmpeg进行编码转换，用到的命令行也不尽相同。[ffmpeg](https://www.ffmpeg.org/)在这里有点大材小用了，[qaac](https://github.com/nu774/qaac/releases)更便捷一些。

大神最新汉化的foobar2000里带有可转换alac的命令行，只需要从上面下载qaac编码器，丢到相应的目录里，就能全自动转换，非常方便，而且tag可以完美的转过来，解开wav与原来的flac解开的wav对比后，MD5也是相同的。

自己打包了一个版本，加上了一个mp3错误校验，需要的朋友可以自行[下载](/uploads/2015/05/foobar2000-v1.6.0.7z)。

---

- 2020-11-17: 更新qaac组件
- 2020-09-30: 更新至1.6.0
- 2017-09-16: 关于摆脱iTunes依赖，请看[这里]({{< relref "qaac-dependency.md" >}})
- 2016-12-29: 更新至1.3.14
- 2015-12-04: 更新解码器并增加x64版本，Win10如果不能转码为Apple Lossless，可以将encoders\qaac.exe和qaac64.exe删除，会自动调用refalac转码
