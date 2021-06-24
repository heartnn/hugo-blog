---
title: 黑群晖在路由器重启后自动获取最新的IPv6并更新DDNS
date: 2021-03-24T09:59:12+08:00
categories: 技术
tags: [synology,ddns,ipv6]
---

首先说明，白群晖也可以这么折腾，但没必要。

下面的问题以及解决方法都是基于`ip addr`的方法更新记录的，如果是访问第三方网站获取本机IP的话，应该不存在这些问题。

起因是由于黑群晖在路由器重启后，前缀发生改变，但旧的IPv6地址并没有释放，所以造成会有很多IPv6的公网地址，当然只有最新的才可以访问到群晖，这时很多DDNS脚本都没有对此的解决方案，一般脚本获取到多个地址时，就会将第一个地址更新到DDNS解析网站上。

虽然新地址与旧地址并没有什么规律可循，但是每次路由器重启后，会分配一个DHCPv6给群晖（我的OpenWrt是通过DHCPv6-PD分配地址的），并且这个后缀一般不会存在多个地址，所以最后就利用这个IPv6来更新DDNS。<!--more-->

这里假设路由器分配给群晖的固定后缀是`::a`、网卡是`ovs_eth0`，那么获取IPv6的命令就是这样的：

    ip addr show ovs_eth0 | grep "inet6.*global" | awk '{print $2}' | awk -F"/" '{print $1}' | grep "::a"

另外一种比较好的解决方法就是利用OpenWrt更新DDNS，采取脚本的方式，得到IPv6前缀，再加上群晖网卡mac生成的后缀，就能得到完整的IPv6地址，获得IPv6前缀的命令如下：

    ubus call network.interface.wan_6 status | grep -A 3 '"ipv6-prefix":' | grep address | awk '{print $2}' | grep -oE '[0-f]{0,4}\:[0-f]{0,4}\:[0-f]{0,4}\:[0-f]{0,4}'

其中，`network.interface.wan_6`可能需要修改，运行`ubus list`可以查找自己需要的接口名称。
