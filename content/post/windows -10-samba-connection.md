---
title: Windows 10 Samba连接问题
date: 2018-08-09T15:13:20+08:00
categories: 技术
tags: [windows,samba]
---

最近重装完系统，发现连接路由器的Samba服务出现问题，自以为华硕RT-AC68U采用的是Samba 2.0，而且路由器设置里也打开了相应的选项，但似乎Windows 10无法识别。

结论是只能启用SMB 1.0了，首先打开控制面板-->程序和功能-->启用或关闭Windows功能，勾选`SMB 1.0/CIFS文件共享支持`。

![](/uploads/2018/08/windows-components.png)<!--more-->

然后运行`services.msc`，确认一下服务开启，并设置为自启动或延时启动：

- DNS Client
- Function Discovery Provider Host
- Function Discovery Resource Publication
- SSDP Discovery
- UPnP Device Host

如果这样还不行，那么就强制设置为旧版的SMB(下面命令需要在管理员的命令提示符下输入)：

```dos
SC.EXE config lanmanworkstation depend=bowser/mrxsmb10/nsi
SC.EXE config mrxsmb20 start=disabled
```
参考：

- https://www.mobile01.com/topicdetail.php?f=300&t=5335865
