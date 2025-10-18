---
title: doubanio反代
date: 2025-10-18T02:08:00+08:00
categories: 网络
tags: [docker]
---

因为一直都是用一个电影信息查询脚本来更新电影信息，都是豆瓣上的信息，海报也是豆瓣的，但是最近几天img9.doubanio.com开始抽风，说什么`错误代码：418 I'm a teapot`，完全不让白嫖了，好在现在AI强大，就撸了一个Cloudflare Workers的代码，但是这东西如果用超了毕竟是要收费的，好在VPS的流量足够多，就直接在VPS中反代就好了。

以前都是用[nginx-proxy](https://github.com/nginx-proxy/nginx-proxy)和[Nginx Proxy Manager](https://nginxproxymanager.com/)，最多用过[caddy-docker-proxy](https://github.com/lucaslorentz/caddy-docker-proxy)，但是这几个东西的主要作用是代理本地的Docker容器，NPM凑合能用，但是搞不定Referer，AI问了一圈，最后自己撸Caddyfile才是王道（主要是Nginx需要自己搞定证书，有点繁琐）。

<!--more-->与Qwen交流之后，得到了一下的代码，以下假设你准备的域名是img*.douban-proxy.com，然后你有一个BBS或者网站叫做bbs.site.com

```yaml
# Global Options
{
    # 设置用于 ACME 证书管理的邮箱地址
    email xxxxx@xxxxx.com
}

(@check_douban_proxy_referer) { # 这段主要是防盗链
    @from_bbs {
        header Referer *bbs.site.com* # 匹配
    }
    @not_from_bbs {
        not header Referer *bbs.site.com* # 不匹配或空Referer
    }
}

img1.douban-proxy.com {
    import @check_douban_proxy_referer
    handle @not_from_bbs {
        respond "Forbidden" 403
    }
    handle @from_bbs {
        reverse_proxy https://img1.doubanio.com {
            header_up Host {upstream_hostport} # 设置正确的 Host 头
            header_up Referer https://www.douban.com # 设置 Referer 头
            header_up X-Real-IP {remote_host}
        }
    }
    header -Server
    header -Via
    header -X-Cache
    header -X-Powered-By
}

img3.douban-proxy.com {
    import @check_douban_proxy_referer
    handle @not_from_bbs {
        respond "Forbidden" 403
    }
    handle @from_bbs {
        reverse_proxy https://img3.doubanio.com {
            header_up Host {upstream_hostport}
            header_up Referer https://www.douban.com
            header_up X-Real-IP {remote_host}
        }
    }
    header -Server
    header -Via
    header -X-Cache
    header -X-Powered-By
}

img9.douban-proxy.com {
    import @check_douban_proxy_referer
    handle @not_from_bbs {
        respond "Forbidden" 403
    }
    handle @from_bbs {
        reverse_proxy https://img9.doubanio.com {
            header_up Host {upstream_hostport}
            header_up Referer https://www.douban.com
            header_up X-Real-IP {remote_host}
        }
    }
    header -Server
    header -Via
    header -X-Cache
    header -X-Powered-By
}
```

然后新建一个Caddy容器，用volume引用这个文件就可以了，既能反代，还可以防盗链。

另外，如果你有别的容器需要代理，完全可以拿这个代替，Caddyfile再多写几个Site Block的事，总体学习起来比Nginx要简单，具体到资源占用和速度，其实不是强迫症的话可以接受，除非你的并发访问非常大。

更多使用方法可以跳转到[Caddy v2 中文文档](https://caddy2.dengxiaolong.com/docs/)。