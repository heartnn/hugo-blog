---
title: "Windows To Go与Linux共存办法"
date: 2015-03-15T01:41:00+08:00
categories: 技术
tags: [linux,"windows to go"]
---

本文旨在提供简单的说明，并不提供具体的实施方案，由于硬件设备不同，本文不具有任何通用的可行性。本文的方法不具有唯一性但简单有效，高手请略过。

首先是对大容量设备的分区， 此处的大容量设备不建议使用U盘，当然64G、128G的USB 3.0另当别论，实际上，一个USB 2.0的移动硬盘就可以工作的很好。分区的时候建议一个主分区安装Windows To Go(以下简称WTG)，然后剩余的做成逻辑分区。

![](/uploads/2015/03/partition.png)<!--more-->

此处需要注意的是我的硬盘中剩余的NTFS大分区是被我当作存储用的，实际在硬盘的末尾，是我在安装LinuxMint前删掉了所有的Linux分区，导致编号改变。实际上分区应该如此，由于某些bios只能识别前127G空间，所以当系统安装在靠后的位置则无法启动(应该是针对老的机器了)，这里用的是MBR分区表，UEFI+GPT请自行Google。

双系统的安装顺序不重要，安装完第二个系统后可能需要修复引导，安装WTG可以使用[WTG辅助工具](http://bbs.luobotou.org/thread-761-1-1.html)。

当其中Linux出现问题时，只需要重装即可，可以对Linux的分区进行任何操作，一般主流Linux(比如Ubuntu或Fedora)都可以识别Windows分区版本。

当WTG出现问题时，重新安装后，可以用EasyBCD添加Linux系统(此处最好记得之前Linux的boot分区)。
