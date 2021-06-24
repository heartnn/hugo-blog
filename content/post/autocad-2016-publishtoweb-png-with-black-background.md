---
title: AutoCAD 2016打印为黑色背景
date: 2018-02-26T17:49:00+08:00
categories: 软件
tags: [autocad]
---

AutoCAD打印中的PublishToWeb PNG默认为白色背景，虽然可以用pngout命令可以生成黑色背景的png文件，但是分辨率却相当可观，网上的教程似乎又不太对，所以只好自己来了。

首先按Ctrl+P打开打印窗口，然后右上角下拉新建打印样式表，此处命名为“黑色背景”，然后点右边的编辑按钮。

![](/uploads/2018/02/autocad-2016-print-with-black-background-1.png)<!--more-->

打印的土建图纸部分，文字本身是白色的，但实际打印的时候会被打印为黑色，所以这里对黑色进行编辑(也就是颜色7)，颜色下拉到选择颜色，白色本来应该是255,255,255的，但似乎系统不允许，所以在这里改成254,254,254，相差一点根本看不出来。

![](/uploads/2018/02/autocad-2016-print-with-black-background-2.png)

保存并关闭后，选择打印机的PublishToWeb PNG.pc3，点右边的特性--&gt;自定义特性，选择背景颜色为黑色。

![](/uploads/2018/02/autocad-2016-print-with-black-background-3.png)

剩下的选项和平时打印为pdf或其它格式一样，可参照第一张图。
