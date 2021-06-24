---
title: 五步大幅提高 Firefox 页面加载速度
date: 2008-12-03T20:50:00+08:00
categories: 软件
tags: [browser,firefox]
---

1. 先在地址栏键入 `about:config`
2. Set `network.http.pipelining` to `true`
3. Set `network.http.proxy.pipelining` to `true`
4. Set `network.http.pipelining.maxrequests` to `30`
5. 单击右键，选择新建 -> 整数,命名为 `nglayout.initialpaint.delay`,值为 `0`

以上部分内容来自 [LinuxSir.Org](http://www.linuxsir.org)。

下面是我的一些实验： 以上的原理就是利用多个线程下载一个页面，30 这个数字如果你的机器和网络够好的话，可以适当再加大些，不过不要超过 64，如果打开页面很多，这个数字超过 64 以后硬盘会不停的响，估计对硬盘有点损坏。

还有就是 `about:config` 这个参数，不要擅自修改里面的其他参数(除非你的确知道你在干什么)，否则 Firefox 就不能好好工作了。
