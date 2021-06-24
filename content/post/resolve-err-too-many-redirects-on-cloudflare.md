---
title: "解决Cloudflare产生的“ERR_TOO_MANY_REDIRECTS”错误"
date: 2018-11-17T10:33:23+08:00
categories: 技术
tags: [dns]
---

昨天打开自己的博客，发现了“ERR_TOO_MANY_REDIRECTS”，估计是最近把DNS改到Cloudflare产生的后果，于是开始排查错误。原来是Cloudflare登录后对应域名的Crypto问题，由于我的域名下有子域名处于http状态，前几天将SSL加密部分从Full SSL改为了Flexible SSL，由此造成的“ERR_TOO_MANY_REDIRECTS”。

问题找到了，由于我的博客托管在Github，首先去掉Github Pages设置里的Enforce HTTPS选项，因为这里http会被强制跳转到https，而Cloudflare的Flexible SSL选项需要阅读原始服务器的http数据，然后进行加密，再传送过来，如果原始服务器返回https的话，Cloudflare会丢弃https数据，然后再继续请求，这就是造成问题的元凶。

![](/uploads/2018/11/github-pages.png)<!--more-->

只是把这个勾去掉，其实还是不行的，似乎是Github有缓存机制，而且不知道要等多长时间，所以还是上面那个Github Pages设置，把Source部分改成别的，然后再改回来，缓存就完全没有了。（浏览器调试的时候可以选择无痕窗口，这样不会带来其它的问题。）

Chrome浏览器提示的“清除Cookies”，其实并没有什么卵用，等你的设置都改好了以后，自然就不会出现问题了。

参考资料：

- [Why does Flexible SSL cause a redirect loop?](https://support.cloudflare.com/hc/en-us/articles/115000219871)
- [What do the SSL options mean?](https://support.cloudflare.com/hc/en-us/articles/200170416-What-do-the-SSL-options-mean-)
