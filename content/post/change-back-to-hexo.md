---
title: 重新换回了Hexo
date: 2017-10-25T19:06:20+08:00
categories: 日常
tags: [hexo]
---

我在这里跟自己说一句：这真是最后一次了~~

还好这次找到个轮子，不过切换过程并不复杂，只要把一开始yaml的内容整理好就可以了，强迫症受不了。。。

基本上属于无损切换，保留了原来的网站格式，所有的文章路径都没有变化，皮肤选用Next，所以评论也从Disqus简单切换到HyperComments，现在可以正常看到评论了。

所有的js库基本都切换到了[CSS.NET](https://www.css.net/)，速度还是非常不错的。

博客部署在Netlify上，和git仓库挂钩，全自动generate，本地甚至可以连node.js都不用装了，而且提供了可以自动更新的[Let's Encrypt](https://letsencrypt.org/)证书，这样的话全站https也没有问题了。
