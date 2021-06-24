---
title: "Fedora 14初级折腾——Flash篇"
date: 2010-11-15T08:32:00+08:00
categories: 技术
tags: [fedora]
---

说实在的，[上次]({{< relref "using-fedora-14.md" >}})只是个安装过程，本来不想再继续折腾了的，但是为了折腾精神，还是打开虚拟机继续了。

打开Konqueror以后，首先碰倒的问题是输入法，不知道是不是脑残的设计，竟然装了中文语言后没有启用输入法。。。

Konqueror实在不是个好的浏览器，多窗口反应不快，而且窗口太多的时候，新开的窗口会没有连接，不知道是不是Fedora的连接数有问题。

于是乎求助于常用的Chrome，在Fedora的Wiki里发现了[Chromium的repo](http://fedoraproject.org/wiki/Chromium)，经过了一番下载后(大概是下载了27MB的数据包，还包含几个fedora自己的库)，终于看到了久违的Chromium，于是一切都习惯了～～<!--more-->

在Ubuntu下已经习惯了Ubuntu Tweak，Fedora下却没有这么好用的工具了。还好Google Code上有[Ailurus](https://code.google.com/p/ailurus/)可以作为替代使用，因为以前heartnn在Ubuntu下也用过这个，所以上手很容易了。

![](/uploads/2010/11/ailurus.jpg)

Fedora下的截图工具也还算好用，只是不能存为gif的，授权问题把。但是往[Sa3album](http://sa3.org/program/gae-album/)上传的时候却是上传界面没有flash支持，64位的Fedora在Adobe上只有源码，哭。。。还好在Ailurus里发现了gnash提醒了我，这个要相对方便多了。但是不知道是不是64位系统的缘故，这个flash插件还是有些水土不服的。显示一般的flash广告没有问题，但是显示[Sa3album](http://sa3.org/program/gae-album/)的时候却不行。建议不爱折腾的同学就这样就好了。

Adobe官方的Flash插件是可以用的，64位是[实验室版](http://labs.adobe.com/downloads/flashplayer10.html)。而且配合Konqueror也无法上传，不得不让我再说一次：[万恶的GAE图床]({{< relref "sa3album-bug.md" >}})。。。于是索性连Firefox也装了上去，结果测试在Chromium和Firefox下都正常了。忘了说一下，插件是tar.gz格式的，解压缩后只是个.so格式的库文件，放到`/usr/lib64/mozilla/plugins`下就可以了。

最后，最合适的组合是Firefox或者Chromium加官方的实验室版Flash，这是针对64位的配置，如果是32位的话，Adobe直接提供rpm包了。
