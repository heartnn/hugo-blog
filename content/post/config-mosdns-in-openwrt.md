---
title: OpenWrt中MosDNS配置
date: 2022-05-25T09:47:08+08:00
categories: 网络
tags: [openwrt,dns]
---

请配合luci-app-mosdns包的自定义配置食用，本配置修改自官方配置luci-app-mosdns包的配置，兼顾了IPv6的功能，但不会碰到那种客户端解析IPv6优先导致超时连接的恶心情况。

使用时注意一下几点，避免踩坑：

1. 端口号如果不是5335的需要自行修改。
2. 配置里的DNS可以使用UDP、DOH、DOT连接，使用DOH和DOT时第一个DNS尽量使用IP，如果一定要使用域名，请参考官方配置对应的IP地址，因为MosDNS可能不能直接解析域名。
3. 与Passwall的配合使用，需要将Passwall的远程DNS设置为`127.0.0.1:5335`或者你的自定义端口号。
4. 保证配置中提到的文件可用，除了luci-app-mosdns包，还需要安装v2ray，安装了Passwall的会自动安装v2ray依赖。

以下是`/etc/mosdns/cus_config.yaml`的内容：<!--more-->

```yaml
log:
  level: error
  file: /tmp/mosdns.txt

plugin:
  - tag: main_server
    type: server
    args:
      entry:
        - _no_ecs
        - lazy_cache
        - _prefer_ipv4
        - _single_flight
        - main_sequence
      server:
        - protocol: udp
          addr: "127.0.0.1:5335"
        - protocol: tcp
          addr: "127.0.0.1:5335"

  - tag: main_sequence
    type: sequence
    args:
      exec:
        - query_is_hosts_domain
        - query_is_redirect_domain
        - if:
            - query_is_whitelist_domain
          exec:
            - forward_local
            - _return
        - if:
            - query_is_blocklist_domain
            - query_is_ad_domain
            - qtype65
          exec:
            - _block_with_nxdomain
            - _return
        - if:
            - query_is_local_domain
            - "!_query_is_common"
          exec:
            - forward_local
            - _return
        - if:
            - query_is_non_local_domain
          exec:
            - forward_remote
            - _return
        - primary:
            - forward_local
            - if:
                - "!response_has_local_ip"
              exec:
                - _drop_response
          secondary:
            - forward_remote
          fast_fallback: 150
          always_standby: true

  - tag: forward_local
    type: fast_forward
    args:
      upstream:
        - addr: https://1.12.12.12/dns-query
        - addr: https://223.5.5.5/dns-query

  - tag: query_is_whitelist_domain
    type: query_matcher
    args:
      domain:
        - "ext:./whitelist.txt"

  - tag: query_is_blocklist_domain
    type: query_matcher
    args:
      domain:
        - "ext:/etc/mosdns/rule/blocklist.txt"

  - tag: query_is_hosts_domain
    type: hosts
    args:
      hosts:
        - "ext:/etc/mosdns/rule/hosts.txt"

  - tag: query_is_redirect_domain
    type: redirect
    args:
      rule:
        - "ext:/etc/mosdns/rule/redirect.txt"

  - tag: forward_remote
    type: fast_forward
    args:
      upstream:
        - addr: https://8.8.8.8/dns-query
        - addr: https://208.67.222.2/dns-query
        - addr: https://doh.dns.sb/dns-query

  - tag: lazy_cache
    type: cache
    args:
      size: 512000
      lazy_cache_ttl: 259200

  - tag: query_is_local_domain
    type: query_matcher
    args:
      domain:
        - "ext:/usr/share/v2ray/geosite.dat:cn"
        - "ext:/usr/share/v2ray/geosite.dat:apple-cn"

  - tag: query_is_non_local_domain
    type: query_matcher
    args:
      domain:
        - "ext:/usr/share/v2ray/geosite.dat:geolocation-!cn"

  - tag: response_has_local_ip
    type: response_matcher
    args:
      ip:
        - "ext:/usr/share/v2ray/geoip.dat:cn"

  - tag: query_is_ad_domain
    type: query_matcher
    args:
      domain:
        - "ext:./serverlist.txt"

  - tag: qtype65
    type: query_matcher
    args:
      qtype: [65]
```
