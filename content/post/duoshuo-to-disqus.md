---
title: 多说关闭，转向Disqus
date: 2017-03-24T12:24:00+08:00
categories: 日常
tags: [duoshuo,disqus,hosts]
---

这两天打开博客一看，发现了多说即将关闭的通告，实在郁闷，又必须迁移评论了，于是乎开始尝试各种社会化评论，顺便把结果记录在这里。

首先是畅言，先不说别的，就备案这一条，我就通不过啊。。。有备案的朋友可以去试试，原来尝试过导入，结果非常蛋疼，现在未知。

其次说说友言，之前多说用uuid来识别，友言没有提供这个功能，只有一个简单的代码，看起来应该是按照url来识别的。

网易云跟帖，有知乎上的网友说不错，可惜我导入多说完全失败，清空也很麻烦，还不让删除，给客服留言，两天没反映，冲这个服务也不敢用啊。。。

最后确定还是Disqus，目前最大的麻烦就是被墙掉了，改hosts还是可以用的。<!--more-->

```plain
151.101.0.134	disqus.com
151.101.24.134	www.disqus.com
151.101.24.134	a.disquscdn.com
108.168.151.3	media.disqus.com
50.18.252.168	help.disqus.com
61.220.62.196	about.disqus.com
61.220.62.196	blog.disqus.com
61.220.62.196	publishers.disqus.com
151.101.24.64	glitter.services.disqus.com
151.101.24.64	links.services.disqus.com
173.192.82.194	realtime.services.disqus.com
151.101.24.134	content.disqus.com
151.101.24.134	referrer.disqus.com
151.101.24.134	docs.disqus.com
151.101.24.134	heartnn.disqus.com
```

P.S 关于导入Disqus，这次是用的[这个轮子](https://urouge.github.io/migrate-to-disqus/)，但是似乎丢掉了些评论，也不想再纠结了，就这样吧。
