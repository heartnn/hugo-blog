---
title: "自己寻找Google Chrome最新版本"
date: 2008-12-27T09:31:00+08:00
lastmod: 2011-06-22
categories: 软件
tags: [browser,chrome]
---

现在大家应该都用过Google浏览器了吧？真的是很稳定的，界面也不错，虽然是刚刚开发出来，功能还不是很多，但是浏览起来速度还是不错的，内存占用相对IE和Firefox都比较小，最重要的是多进程，呵呵，这个功能IE8才有的。

闲话少说，Google Chrome的开发版本叫做Chromium，这个可以说是经常更新的，所以大可不必经常去下载，不过它相对于Chrome来说，[Acid Test 3](http://acid3.acidtests.org/)测试结果是100%，这个结果和Opera10是相同的哦。

要想查看Chromium的最新版本，不妨到下面这个地址：<http://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?path=Win/&amp;sort=desc>（2012-02-29更新地址）

其中版本号最大的那个就是最新的了，下载其中的`chrome-win32.zip`就可以了。

如果你觉得还是麻烦的话，那么在下提供个工具吧 ^_^ 一个是[Chromium Nightly Updater](http://www.donationcoder.com/Forums/)，这个网上说失效了？可是heartnn现在测试还是可以的。另一个是[muldeR](http://mulder.dummwiedeutsch.de/home/)做的[Chromium Updater](http://mulder.dummwiedeutsch.de/home/?page=projects#chromium)，这个可以放到Chromium的文件夹中，用来检测当前是否需要更新。<!--more-->

好了，接下来，你一定比较感兴趣的就是这么绿化了吧，这里可以用[PortableappZ的程序](/uploads/2008/12/chromeportable.7z)，还有就是自己动手：

在chrome.exe所在的目录下建立一个批处理文件，写上下面的内容

```dos
start "" chrome.exe --user-data-dir="%~dp0profile" --single-process --disable-java --lang=zh-CN
```

具体说一下这些参数的作用吧：

 - `--user-data-dir="%~dp0profile"`的作用就是在chrome.exe所在的目录下建立一个叫profile的目录，用来存放你的信息，比如浏览记录和cookie等。
 - `--single-process`看名字就知道了，是用单进程，不过这样做的话崩溃的时候就全关了。。。
 - `--disable-java`禁止Java的，[据说](http://initiative.yo2.cn/archives/631785)Google Chrome的[第二个安全漏洞](http://raffon.net/research/google/chrome/carpet.html)就是Java的，heartnn没有仔细看。
 - `--lang=zh-CN`这个不多说了，中文化。有中文没人想看英文吧？（什么？你练英语？怎么不再试试别的语言。。。）

其实，只是一般用的话，只用`--user-data-dir="%~dp0profile"`就可以了。

到这里，大家应该都有自己的绿色Chrome了吧。

2011-06-22: 修正了Chromium的更新地址。
