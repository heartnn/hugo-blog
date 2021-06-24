---
title: 从零开始折腾梅林并安装syncthing
date: 2017-11-23T17:01:55+08:00
category: 技术
tags: [merlin,syncthing]
---

双11入手了一台美版华硕AC68u(美版名字T-Mobile AC-1900)，正好家里还有[以前那个移动硬盘]({{< relref "linux-portable.md" >}})，于是乎重头开始了折腾。

首先是移动硬盘的格式化，建议就不用分区了吧，个人感觉格式化成ext4要比ntfs更好一些，在访问上也没问题，这个以后慢慢说。

由于格式化的时候填写了卷标为asus，所以挂载到路由器上的路径就是`/mnt/asus`，这便是挂载的根目录了。

然后打开jffs分区，以便保存运行的脚本设置，使用软件中心的话也必须打开jffs分区：<!--more-->

![](/uploads/2017/11/enable-jffs.png)

然后列出系统可用空间：

![](/uploads/2017/11/df.png)

由于heartnn家里的宽带没有公网IP(黑心的联通)，所以DDNS暂时是没法直接使用呢，这个可以慢慢解决，现在可以先装个可以穿透内网的[syncthing](https://syncthing.net)

syncthing用来做多个设备之间的同步很好用，私以为比Resilio Sync要好用一些，关键是开源。

先在移动硬盘中建立文件夹syncthing，可在路由器设置界面中的`USB相关应用--\>服务器中心--\>网络共享（Samba）`中建立，或者通过ssh建立。

从<https://github.com/syncthing/syncthing/releases/latest>下载arm的版本的syncthing，然后解压缩出syncthing主程序，放到`/mnt/asus/syncthing/bin`，如果需要[syncthing-inotify](https://github.com/syncthing/syncthing-inotify/releases/latest)的话也一并下载，然后在ssh中执行：

```bash
cd /mnt/asus/syncthing/bin
chmod +x syncthing*
```

为了方便管理，syncthing的配置文件最好也在移动硬盘中(`/mnt/asus/syncthing/config`)，这里建立一个`syncthing.sh`放在相同目录下，内容为：

```bash
#!/bin/sh

/mnt/asus/syncthing/bin/syncthing -no-browser -home=/mnt/asus/syncthing/config -logflags=0 >/dev/null &
```

日志的话就丢到黑洞里不要了，`&`的作用是保持syncthing后台运行，在ssh中执行`top`命令可以看到相关进程。

对应的syncthing-inotify由于配置目录的移动，只能以api形式运行，建立`syncthing-inotify.sh`：

```bash
#!/bin/sh

/mnt/asus/syncthing/bin/syncthing-inotify -api=""xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"" -logflags=0 >/dev/null &
```

请自行替换为自己的api。

运行成功后，到`/mnt/asus/syncthing/config`中编辑config.xml，将`<address>127.0.0.1:8384</address>`修改为`<address>0.0.0.0:8384</address>`，这样保证以后在局域网内可以进行管理。

kill掉syncthing的进程重新启动，然后应该可以在浏览器进行管理了，之后最好是在syncthing管理界面中设置用户名和密码。

将两个脚本加入`/jffs/scripts/post-mount`中可以实现自启动。这里实现自启动网上有多种说法，这是比较简单的一种，也不用依赖什么服务。

```bash
sh /mnt/asus/syncthing/bin/syncthing.sh
sh /mnt/asus/syncthing/bin/syncthing-inotify.sh
```

这样即使重启路由器，syncthing也能顺利的跑起来了，但是有一个缺点，会收到一个提示：

> Syncthing should not run as a privileged or system user. Please consider using a normal user account.

这个据[论坛说法](https://forum.syncthing.net/t/the-meaning-of-syncthing-should-not-run-as-a-privileged-or-system-user-please-consider-using-a-normal-user-account/10034/2)是无法去掉的，不过heartnn总是有办法，咱们下回再说。
