---
title: "在vps上搭建frp"
date: 2018-03-23T19:51:44+08:00
categories: 技术
tags: [frp,vps]
---

[frp](https://github.com/fatedier/frp)是一款内网穿透工具，在现在宽带没有公网IP的年代，NAS服务器之类的，必须使用内网穿透来解决WAN下的访问问题。

网上有很方便的[一键安装脚本](http://koolshare.cn/thread-65379-1-1.html)，用于VPS上安装frp。

这里主要说一下服务器以及客户端的配置：

首先是frps.ini，这个文件一般是脚本配置后自动生成的，这里注释说明一下并稍微添加修改：<!--more-->

```ini
[common]
bind_addr = 0.0.0.0 # 可以访问frp的目标IP地址，0.0.0.0为不限制
bind_port = 5443 # tcp主要端口
kcp_bind_port = 5443 # kcp端口(使用的是udp)，可以和tcp端口相同
bind_udp_port = 7001 # 打开一个udp端口，以便使用p2p等功能
dashboard_port = 6443 # web控制台访问端口，如果不打算用控制台，可以不用设置
dashboard_user = admin # web控制台的用户名，建议更改
dashboard_pwd = xxxxxxxx # web控制台的登录密码
vhost_http_port = 82 # 如果不打算在VPS上架设网站，这里可以用80端口
vhost_https_port = 444 # https的代理端口，需要证书，配置较复杂，如果VPS上不架设网站，这里可以用443端口
log_file = /dev/null # 不生成log
log_level = error
log_max_days = 3
privilege_token = xxxxxxxxxxxxxxxx # 这里是认证令牌，自用的话必须设置
privilege_allow_ports = 1-65535 # 可限制frps使用的端口范围，该范围需要在VPS的防火墙中打开
max_pool_count = 50 #自用的话50足够了
tcp_mux = true
subdomain_host = xxx.com #这里可以用A记录绑定VPS的IP
```

下面是关于客户端的配置，假设有一台路由器A以及一台位于其他位置的电脑B，其中A为24小时开机，A内设有Syncthing用来同步其他设备，并安装sftp。

现在需要开放A中的Syncthing、sftp、ssh供B访问(其中sftp与ssh端口相同)，需要进行的frpc.ini配置：

```ini
[common]
server_addr = xxx.xxx.xxx.xxx # VPS的IP或绑定的域名
server_port = 5443
privilege_token = xxxxxxxxxxxxxxxx # 之前frps.ini中对应的令牌
protocol = kcp # 这里可以是tcp或kcp

[ssh]
type = tcp # 这里不要因为前面设置了kcp就填上kcp
local_ip = 127.0.0.1 # 因为是本机，所以这样就可以了
local_port = 22 本地路由器A的ssh端口，因为会暴露出来，建议从该设备中更改端口
remote_port = 50022 # 会打开新的VPS端口，请确定防火墙通过
use_encryption = false # 是否加密，一般不需要
use_compression = false # 是否压缩，会影响CPU使用

[sync]
type = http
local_ip = 127.0.0.1
local_port = 8384 # Syncthing的端口
use_encryption = false
use_compression = true
subdomain = sync # 这里如果不指定，将使用上面方括号内的服务名为二级域名；如果VPS中没指定subdomain_host，则这里需要用custom_domains来指定一个访问域名，该域名应当解析到VPS上

[p2p_ssh] # 这里配置的p2p是可以穿透VPS，让两台设备直接相连，这才是我想要的
type = xtcp # 会使用上面的7001端口
sk = heartnn # 这里的字符也会在电脑B中设置，类似于令牌之类的
local_ip = 127.0.0.1
local_port = 22

```

经过设置，可以在任意设备中输入`http://sync.xxx.com:82`来访问Syncthing的控制台，也可以用xxx.com:50022来访问

电脑B中的frpc.ini

```ini
[common] # 与上面相同即可
server_addr = xxx.xxx.xxx.xxx
server_port = 5443
privilege_token = xxxxxxxxxxxxxxxx
protocol = kcp

[p2p_ssh_visitor] # 这里是上面对应的p2p访问者配置
type = xtcp
role = visitor # 访问者
server_name = p2p_ssh # 对应A中的服务名称
sk = heartnn
bind_addr = 127.0.0.1 # 在电脑B本地访问
bind_port = 50022 # 这里是电脑B本地端口，与其他无关
```

经过设置，B与A可以通过p2p直连，heartnn测试过sftp完全没问题，但ssh没能通过p2p，只能过VPS中转，不过已经完全够用了。