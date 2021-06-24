---
title: "使用Zerotier为Syncthing打洞"
date: 2018-04-03T19:55:11+08:00
categories: 技术
tags: [zerotier,syncthing]
---

最近[Syncthing](https://syncthing.net)越来越慢，有时竟无法连接，翻看官网的[github](https://github.com/syncthing/syncthing)，发现是去掉了kcp，老外竟然说kcp不如tcp快，这真是不符合我国的网络啊。。。

起初换了[微力同步](http://verysync.com/)，本以为中继服务器在国内，应该好一点吧，谁知道依然是那个样子，毕竟是Syncthing改的，改成Resilio Sync的样子已经不错了，不适合我这种nat到nat的网络，而且没有版本控制，绝对的适合文件分享，而不是文件同步用。

换回Syncthing的过程又折腾了一次，正赶上VPS抽风，frp也不给力了，好在发现了[Zerotier](https://www.zerotier.com/)这款神器，正好解了燃眉之急。

Zerotier属于虚拟局域网，可以把不同网络状态下的多个设备组织在一起，正好为我所用。正常注册Zerotier的free账户，然后登陆后，可以在Networks下Create一个新的局域网，会得到一个Newwork ID，设备会依靠这个ID来识别属于哪个局域网，最后是把每一台需要连接的设备都安装Zerotier客户端。<!--more-->

Zerotier的安装很简单，关于Windows和Android不多说，直接下载安装就可以了，以下单独说一下我用的RT-AC68U：

这台设备买到手就刷了梅林，自从上次折腾完[frp]({{< relref "deploy-frp-on-vps.md" >}})以后，就一直在那里吃灰了，登录上ssh，然后执行更新并安装Zerotier：

```bash
opkg update
opkg upgrade
opkg install zerotier
```

过程很简单，但配置稍微复杂一些，先让Zerotier运行起来：

```bash
modprobe tun
zerotier-one -d
zerotier-cli info    # 显示信息
```

会得到`200 info 2********6 1.2.4 ONLINE`，这样就显示这台设备上线了。

然后执行`zerotier-cli join a**************d`，加入之前创建的网络，会提示`200 join OK`。

从网页进入刚才创建的网络，右边可以选择IP段，我自定义输入的`10.10.10.0/24`，只是为了好记，再下面会显示设备列表，勾选Auth?表示同意加入网络，旁边也是可以自己定义ip地址，我是在上面把ipv6关掉了，因为一般用不到。

![](/uploads/2018/04/zerotier-webconfig.png)

验证客户端是否连接成功还可以用`zerotier-cli listnetworks`命令。

这时可以测试一下路由器通不通，不过由于路由器可能是禁ping的，所以不通也不用担心，先往下说。

查看路由器的nat映射：

```bash
iptables -v -L INPUT -n --line-numbers
```

这里列出的内容不重要，关键是那个行号，因为要在下面添加一行：

```bash
iptables -I INPUT 12 -i zt0 -j ACCEPT    # 这里假定原来的iptables有11条信息，所以这里应该是12行，zt0是Zerotier的虚拟网卡
```

成功与否可以再次运行`iptables -v -L INPUT -n --line-numbers`查看，也可以到梅林的管理界面看系统记录里的路由表。

接下来就是防火墙，这里heartnn与原文不太一样，只是不想折腾iptables了，直接关掉了防火墙，建议需要防火墙的同学一定要仔细配置iptables，相关的映射命令如下：

```bash
iptables -t nat -A PREROUTING -d 10.10.10.2 -j DNAT --to-destination 192.168.1.1
```

如果不想整个设备映射，只映射个别端口的话：

```bash
iptables -t nat -A PREROUTING -d 10.10.10.2 -p tcp --dport 80 -j DNAT --to-destination 192.168.1.1:80
```

另外只有这些是不够的，需要设置iptables打开相应的端口才可以，熟悉iptables的自然不用多说，不太会的建议多Google一下。

最后是搞定自启动，这里提供相关的脚本，这里启动脚本只写出关于Zerotier的部分，不要完全照抄就是了：

/jffs/scripts/init-start

```bash
#!/bin/sh
modprobe tun
```

/jffs/scripts/wan-start

```bash
#!/bin/sh
cru a ZeroTierDaemon "* * * * * /opt/etc/init.d/S90zerotier-one.sh start"
```

/opt/etc/init.d/S90zerotier-one.sh

```bash
#! /bin/sh
case "$1" in
  start)
    if ( pidof zerotier-one )
    then echo "ZeroTier-One is already running."
    else
        echo "Starting ZeroTier-One" ;
        /opt/bin/zerotier-one -d ;
        echo "$(date) Started ZeroTier-One" >> /opt/var/log/zerotier-one.log ;
    fi
    ;;
  stop)
    if ( pidof zerotier-one )
    then
        echo "Stopping ZeroTier-One";
        killall zerotier-one
        echo "$(date) Stopped ZeroTier-One" >> /opt/var/log/zerotier-one.log
    else
        echo "ZeroTier-One was not running" ;
    fi
    ;;
  status)
    if ( pidof zerotier-one )
    then echo "ZeroTier-One is running."
    else echo "ZeroTier-One is NOT running"
    fi
    ;;
  *)
    echo "Usage: /etc/init.d/zerotier-one {start|stop|status}"
    exit 1
    ;;
esac

exit 0
```

顺便把防火墙也贴过来，不过是个不完善的版本(这就是我为什么关闭防火墙了)，有时间再不断完善吧。

/jffs/scripts/firewall-start

```bash
#!/bin/sh
logger -t "custom iptables" "Enter" -p user.notice
iptables -C INPUT -i zt0 -j ACCEPT
if [ $? != 0 ]; then
  #iptables -I INPUT -i zt0 -j ACCEPT
  #iptables -I INPUT -i zt0 -p icmp -j ACCEPT
  iptables -I INPUT 1 -i ppp0 -p icmp -j DROP
  iptables -t nat -A PREROUTING -d 10.10.10.2 -p tcp --dport 80 -j DNAT --to-destination 192.168.1.1:80
  logger -t "custom iptables" "rules added" -p user.notice
else
  logger -t "custom iptables" "rules existed skip" -p user.notice
fi
```

最后就是重启设备，享受局域网的快乐~~

参考：

1. https://www.snbforums.com/threads/a-guide-about-installing-zerotier-on-asus-ac68u-router.42648/
2. http://koolshare.cn/forum.php?mod=viewthread&tid=134930
