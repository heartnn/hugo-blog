---
title: 评论系统从多说迁移到Disqus
date: 2016-01-05T09:20:24+08:00
categories: 日常
tags: [python,disqus,duoshuo]
---

最近花了点时间解决了一些个人博客的遗留问题，主要是博客评论和图床的一些问题。

启用多说还要追溯到Hexo的时候，那时切换到了静态博客，就只能使用社会化评论系统了。然后被很多博客忽悠着用了多说，事实上那时的多说还是很好用的，但目前似乎是无人负责的状态。

由于之前从Hexo到Ghost的时候，由于文章slug改变过，所以干脆导入了一个新的多说域名，但事实证明了这是非常错误的，因为多说把所有评论的parent全搞丢了，而且经过导入导出，用户头像也没有了。。。

导入Disqus的过程其实很享受，因为有高人将轮子贴到了[github](https://github.com/JamesPan/duoshuo-migrator)上，中间用到了lxml，建议不要自己编译(因为我装上了VC for Python27也没有编译成功)，而是直接在网上找[非官方的扩展](http://www.lfd.uci.edu/~gohlke/pythonlibs/)。

图片和附件干脆还是放到qiniu上了，原来用Hexo的时候，是不能避免git的，所以图片也都用git一起发布了。但Ghost是可以在线上传图片的，所以为了文章编辑方便，还是启用了Ghost的qiniu插件。
