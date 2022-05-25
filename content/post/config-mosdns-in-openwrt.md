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
  file: '/tmp/mosdns.txt'

plugin:

  ################# 服务器插件 ################

  - tag: main_server    # 服务器插件。接收客户端的请求。
    type: server
    args:
      entry:            # 设定客户端的请求会经过哪些插件(被哪些插件处理)。
        - _no_ecs
        - _single_flight
        - main_sequence    # 运行主执行序列
        - modify_ttl       # 修改应答的 ttl
      server:
        - protocol: udp
          addr: '127.0.0.1:5335'
        - protocol: tcp
          addr: '127.0.0.1:5335'
        - protocol: udp
          addr: '[::1]:5335'
        - protocol: tcp
          addr: '[::1]:5335'

  - tag: main_sequence
    type: sequence
    args:
      exec:

        - if:
            - query_is_blocklist_domain    # 黑名单域名
            - query_is_ad_domain           # 已知的广告域名
          exec:
            - _block_with_nxdomain         # 生成 NXDOMAIN 应答
            - _return                      # 返回。不再执行后续插件。

        - mem_cache    # 运行缓存插件。放在这里就可以避免缓存到无用的广告域名。

        - query_is_hosts_domain

        - query_is_redirect_domain

        - if:
            - query_is_whitelist_domain
          exec:
            - _prefer_ipv4               # 优先 IPv4
            - forward_local
            - _return

        - if:
            - query_is_local_domain    # 已知的本地域名
            - '!_query_is_common'      # 和不常见的请求类型
          exec:
            - _prefer_ipv4             # 优先 IPv4
            - forward_local            # 用本地服务器获取应答
            - _return

        - if:
            - query_is_non_local_domain    # 已知的非本地域名
          exec:
            - _prefer_ipv4                 # 优先 IPv4
            - forward_remote               # 用远程服务器获取应答
            - _return

        # 剩下的未知域名用 IP 分流。原理类似 ChinaDNS。
        # 这里借助了 `fallback` 工作机制。分流原理请参考 `fallback` 
        # 的工作流程。
        - primary:
            - forward_local    # 本地服务器获取应答。
            - if:
                - '!response_has_local_ip'    # 应答包含非本地 IP。
              exec:
                - _drop_response    # 丢掉。
          secondary:
            - _prefer_ipv4        # 优先 IPv4
            - forward_remote      # 用远程服务器获取应答
          fast_fallback: 150      # 这里建议设置成 local 服务器正常延时的 2~5 倍。
                                  # 这个延时保证了 local 延时偶尔变高时，其结果不会被 remote 抢答。
                                  # 如果 local 超过这个延时还没响应，可以假设 local 出现了问题。
                                  # 这时用就采用 remote 的应答。单位: 毫秒。
          always_standby: true

  # 缓存
  - tag: 'mem_cache'
    type: 'cache'
    args:
      size: 512000
      lazy_cache_ttl: 86400

  # 修改应答 ttl
  - tag: 'modify_ttl'
    type: 'ttl'
    args:
      minimal_ttl: 300
      maximum_ttl: 3600

  # 转发请求至本地服务器的插件
  - tag: 'forward_local'
    type: 'fast_forward'
    args:
      upstream:
        - addr: 'https://doh.pub/dns-query'
          dial_addr: '1.12.12.12:443'
        - addr: 'https://dns.alidns.com/dns-query'
          dial_addr: '223.6.6.6:443'
          trusted: true

  # 转发请求至远程服务器的插件
  - tag: forward_remote
    type: 'fast_forward'
    args:
      upstream:
        - addr: 'https://dns.google/dns-query'
          dial_addr: '8.8.4.4:443'
        - addr: 'https://208.67.222.2/dns-query'
          dial_addr: '208.67.222.2:443'
          trusted: true
        - addr: 'https://doh.dns.sb/dns-query'
          trusted: true
        - addr: 'https://doh.cloudflare-dns.com/dns-query'
          trusted: true

  ################ 匹配器插件 #################

  - tag: 'query_is_whitelist_domain'
    type: 'query_matcher'
    args:
      domain:
        - 'ext:./whitelist.txt'

  - tag: 'query_is_blocklist_domain'
    type: 'query_matcher'
    args:
      domain:
        - 'ext:/etc/mosdns/rule/blocklist.txt'

  - tag: 'query_is_hosts_domain'
    type: 'hosts'
    args:
      hosts:
        - 'ext:/etc/mosdns/rule/hosts.txt'

  - tag: 'query_is_redirect_domain'
    type: 'redirect'
    args:
      rule:
        - 'ext:/etc/mosdns/rule/redirect.txt'

  # 匹配本地域名的插件
  - tag: 'query_is_local_domain'
    type: 'query_matcher'
    args:
      domain:
        - 'ext:/usr/share/v2ray/geosite.dat:cn'
        - 'ext:/usr/share/v2ray/geosite.dat:apple-cn'

  # 匹配非本地域名的插件
  - tag: 'query_is_non_local_domain'
    type: 'query_matcher'
    args:
      domain:
        - 'ext:/usr/share/v2ray/geosite.dat:geolocation-!cn'

  # 匹配本地 IP 的插件
  - tag: 'response_has_local_ip'
    type: 'response_matcher'
    args:
      ip:
        - 'ext:/usr/share/v2ray/geoip.dat:cn'

  # 匹配广告域名的插件
  - tag: 'query_is_ad_domain'
    type: 'query_matcher'
    args:
      domain:
        - 'ext:./serverlist.txt'
```
