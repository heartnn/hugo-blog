---
title: 在Android手机中搭建Dnsmasq后续
date: 2015-12-20T01:21:15+08:00
lastmod: 2016-03-19T11:22:58+08:00
categories: 技术
tags: [android,dnsmasq]
---

[之前]({{< relref "dnsmasq-on-android.md" >}})写了一篇关于如何在Android手机中搭建Dnsmasq的文章，使用起来比较繁琐，现在做一个详细的整理。

### 准备工作

下载dnsmasq.zip备用。(链接: https://pan.baidu.com/s/1hrG4GQw 密码: k8kg)

### 测试系统是否支持init.d

将下面的代码保存为`00test`，放置于`/system/etc/init.d`目录。<!--more-->

```bash
#!/system/bin/sh

#Init.d Test
if [ -e /data/Test.log ]; then
rm /data/Test.log
fi

echo  Init.d is working !!! >> /data/Test.log
```

重启手机，如果能看到`/data/Test.log`文件存在，则支持init.d，可直接卡刷dnsmasq.zip

### 添加init.d

1. 如果不愿意折腾，可以直接使用[Kernel Adiutor](http://coolapk.com/apk/com.grarak.kerneladiutor)，打开里面的模拟init.d功能，是一样的，愿意折腾的继续看。

2. 安装[Busybox](http://coolapk.com/apk/stericson.busybox.donate)，打开获取su权限后直接install即可，然后可以随意卸载

3. 确认手机中有终端模拟器，没有戳这里: <http://coolapk.com/apk/jackpal.androidterm>

4. 下载[term-init.sh](/uploads/2015/12/term-init.sh)放于sd卡目录备用。(作者有相关的其他方法，[见XDA原帖](http://forum.xda-developers.com/showthread.php?t=1933849)，不过我还是推荐这个方法，在我的5.1.1原生系统下成功)

5. 打开终端，输入

```bash
su
cd /sdcard #此步骤对应term-init.sh的路径
sh term-init.sh #按操作提示
```

完成后重启，再验证data/Test.log是否存在，如仍然不存在，就只能使用[Kernel Adiutor](http://coolapk.com/apk/com.grarak.kerneladiutor)软件模拟init.d。

### 卡刷dnsmasq.zip

将需要使用的`dnsmasq.conf`替换掉zip里的默认配置(路径在`/data/local`)，然后刷入。不用担心替换系统文件，我已经在脚本中备份系统原dnsmasq。如果需要卸载，那么照着zip中的文件路径删掉即可。

### 用[Override DNS](http://coolapk.com/apk/net.mx17.overridedns)设置DNS

将主dns设为`127.0.0.1`，副dns空，然后开关wifi即可生效。
