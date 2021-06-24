---
title: 在Amazon Lightsail部署Shadowsocks
date: 2018-05-11T19:15:23+08:00
categories: 网络
tags: [shadowsocks]
---

亚马逊的Lightsail首月免费实在是非常诱人，而且东京的节点速度还是不错的，用来部署SS这类工具实在是方便，作为自用是非常实用的。

打开[Lightsail首页](https://lightsail.aws.amazon.com/ls/webapp/home)，首先是选择512M那个首月免费的版本，选择地区，然后我用的是Ubuntu 16.04，为什么选这个，因为安装bbr很方便。

![](/uploads/2018/05/lightsail-create-instance-1.png)<!--more-->

这里如果是首次使用，需要创建SSH密钥。

![](/uploads/2018/05/lightsail-create-instance-2.png)

创建完之后，点击已经启动的实例，可以通过浏览器直接连接SSH，或者使用SSH客户端导入密钥之后使用。

进入SSH以后，首先是系统升级到最新(如果使用浏览器直接连接SSH，这里是可以复制粘贴的)：

```bash
sudo apt update && sudo apt upgrade -y
```

然后开启BBR，先安装Hardware Enablement Stack，将内核更新到最新版：

```bash
sudo apt install --install-recommends linux-generic-hwe-16.04
sudo apt autoremove  # 重启后可选择删除旧内核
```

重启后执行`uname -r`，查看内核版本是否大于4.9，如果是则为成功，可以继续进行。

以下步骤请通过`su`切换为root账户继续执行(可以先通过`sudo passwd root`来更改root账户的密码)：

执行`lsmod | grep bbr`，此时结果中应该没有`tcp_bbr`，然后执行：

```bash
modprobe tcp_bbr
echo "tcp_bbr" >> /etc/modules-load.d/modules.conf
echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
sysctl -p
```

验证：

```bash
sysctl net.ipv4.tcp_available_congestion_control
sysctl net.ipv4.tcp_congestion_control
```

如果上面的执行结果都包含`bbr`，则说明内核开启bbr成功。

可再次执行`lsmod | grep bbr`，查看是否有`tcp_bbr`模块。

最后使用[Shadowsocks一键安装脚本](https://teddysun.com/486.html)，依旧是使用root账户，执行：

```bash
wget --no-check-certificate -O shadowsocks-all.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-all.sh
chmod +x shadowsocks-all.sh
./shadowsocks-all.sh 2>&1 | tee shadowsocks-all.log
```

如果提示没有wget，可以先执行`apt install wget`，安装时选择Shadowsocks-libev版，加密方式可以选择`aes-256-gcm`或`xchacha20-ietf-poly1305`。

另外安装时最好安装[simple-obfs](https://teddysun.com/511.html)，混淆选http还是tls可随意。(这里执行`autoconf --version`查询版本应该是没有问题的，所以可以正常安装。)

参考：

1. [开启TCP BBR拥塞控制算法](https://github.com/iMeiji/shadowsocks_install/wiki/开启TCP-BBR拥塞控制算法)
