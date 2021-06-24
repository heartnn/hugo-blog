---
title: "三星TF卡EVO Plus版本测速"
date: 2019-04-02T10:20:06+08:00
categories: 日常
tags: [tf,samsung]
---

![](/uploads/2019/04/samsung-tf-evo-plus-128g.jpg)

就是这张卡了，根据网上的许多评测，不吹不黑，这张似乎是性价比最高的了，实际测试中也是比较满意的，测试使用的[绿联USB 3.0读卡器](https://item.jd.com/25461204486.html)，完全没有任何问题。

1\. ATTO测试结果，附[BMK文件](/uploads/2019/04/samsung-tf-128g.bmk)：<!--more-->

![](/uploads/2019/04/atto-screenshot.png)

2\. Crystal测试结果：

```
-----------------------------------------------------------------------
CrystalDiskMark 6.0.0 x64 (C) 2007-2017 hiyohiyo
                          Crystal Dew World : https://crystalmark.info/
-----------------------------------------------------------------------
* MB/s = 1,000,000 bytes/s [SATA/600 = 600,000,000 bytes/s]
* KB = 1000 bytes, KiB = 1024 bytes

   Sequential Read (Q= 32,T= 1) :    91.559 MB/s
  Sequential Write (Q= 32,T= 1) :    85.101 MB/s
  Random Read 4KiB (Q=  8,T= 8) :     7.892 MB/s [   1926.8 IOPS]
 Random Write 4KiB (Q=  8,T= 8) :     3.173 MB/s [    774.7 IOPS]
  Random Read 4KiB (Q= 32,T= 1) :     7.275 MB/s [   1776.1 IOPS]
 Random Write 4KiB (Q= 32,T= 1) :     3.017 MB/s [    736.6 IOPS]
  Random Read 4KiB (Q=  1,T= 1) :     5.297 MB/s [   1293.2 IOPS]
 Random Write 4KiB (Q=  1,T= 1) :     2.260 MB/s [    551.8 IOPS]

  Test : 1024 MiB [I: 11.7% (14.0/119.2 GiB)] (x5)  [Interval=5 sec]
  Date : 2019/04/01 11:33:09
    OS : Windows 10  [10.0 Build 14393] (x64)
  
```

![](/uploads/2019/04/crystaldiskmark-screenshot.png)

实际使用中放到华为M3平板里遇到瓶颈了，用ES File复制文件的写入速度基本只有40mb左右，看来是偷工减料了，如果满速的话应该和内置的emmc差距不大。
