---
title: 在Android手机中搭建Dnsmasq
date: 2015-04-26T03:53:00+08:00
lastmod: 2015-12-20
categories: 手机
tags: [android,dnsmasq]
---

网上很久以前就有了在Android下搭建Dnsmasq的方法，不过至少都是两三年前的了，其中也不乏一些编译的方法，需要下载Android源代码，在Linux环境下进行，使得很多人望而却步。

然而heartnn在逛XDA的时候发现[某大神发布的已经编译好的版本](http://forum.xda-developers.com/showthread.php?t=1658598)，虽然也不是很新的(我甚至不知道版本)，但必须尝试一下。

需求：Android任意版本(我是在5.1下测试成功的)，手机需root，内核最好支持init.d，如果不支持的话请使用终端或RE管理器启动97dns。

副作用：替换系统自带Dnsmasq后可能会引起手机自建热点不正常。

需要说明的是，我并没有按照压缩包里的批处理安装，而是自己手动安装的，因为这样的话可以知道文件的去处，方便卸载。<!--more-->

| 文件名 | 目标目录 | 权限 | 用途 |
| --- | --- | --- | --- |
| 97dns | /system/etc/init.d/ | 755 | 启动脚本 |
| dnsmasq | /system/bin/[^1] | 755[^2] | 主文件 |
| dnsmasq.conf | /data/local/ | 644 | 配置文件 |
| dnsmasq-host | /data/local/ | 644 | 额外的hosts[^3] |
| Install.bat | Null | Null | 作者提供的ADB安装脚本 |
| resolv.conf | /system/etc/ | 644 | NameServer定义文件 |

[^1]: 如果目标目录存在同名文件，建议备份
[^2]: 需要修改文件的用户组为2000 - Shell
[^3]: 该文件的位置可从dnsmasq.conf定义，从而使用系统自带的hosts

当一切都完成后，重启手机，切换DNS为127.0.0.1即可，具体Dnsmasq都能干些什么，请自行Google。

下面的部分是对97dns的补充，但有时完全不起作用，当这些代码无效时，建议使用[Override DNS](http://coolapk.com/apk/net.mx17.overridedns)进行修改。

```plain
#KitKat
ndc resolver setifdns rmnet0 "" 127.0.0.1 114.114.114.114
ndc resolver setifdns wlan0 "" 127.0.0.1 114.114.114.114
ndc resolver setifdns eth0 "" 127.0.0.1 114.114.114.114
ndc resolver setdefaultif eth0
#Lollipop
ndc resolver setnetdns rmnet0 "" 127.0.0.1 114.114.114.114
ndc resolver setnetdns wlan0 "" 127.0.0.1 114.114.114.114
ndc resolver setnetdns eth0 "" 127.0.0.1 114.114.114.114
```

最后提供heartnn打包的版本(已更新为[卡刷包]({{< relref "dnsmasq-on-android-part-2.md" >}}))，轻微修改了一下配置文件。

注: 当dnsmasq出现问题，比如自己配置的hosts出现问题，需要使用其它穿越软件的时候，记得使用[Override DNS](http://coolapk.com/apk/net.mx17.overridedns)切换DNS，否则可能会造成其它软件出现问题。

- 2015-07-27: 更改DNS转发为AliDNS
- 2015-07-21: 更改DNS转发为OneDNS，不再使用114DNS
- 2015-12-20: 将Dnsmasq制作卡刷包
