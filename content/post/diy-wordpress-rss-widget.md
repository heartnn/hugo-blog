---
title: 自己打造Wordpress的侧边栏RSS订阅图标
date: 2010-12-05T01:02:00+08:00
categories: 技术
tags: [rss]
---

![](/uploads/2010/12/rss-sign.png)

这两天换了主题，想顺便也加强一下RSS订阅吧，于是开始从网上搜索各种插件，结果没有让我感到满意的，不是链接第三方JS，就是自定义功能不强，心想还是自己手工制作吧。

懒了很久了，自己的制作水平也差了很多，本来很简单的东西折腾了很久。。。

首先找到一套订阅图标，尽量一致的风格(这个为了美观，你懂的)，然后对每个图像书写链接，这是最简单的办法，例子就不给了，仅使用html代码就能搞定，唯一的缺点是对每个图像都会有一次http请求，感谢速度会比较慢。<!--more-->

接下来就发挥我们的折腾精神吧，把所有图标都合并成一个文档，(这里heartnn走了不少弯路，一开始用了background-position，可是发现无法做成链接)利用map标签，用css就可以搞定了，下面是heartnn的代码，值得注意的是我这里用的是矩形热点，coords属性对应的是两个对角的坐标，map标签中的name和id要对应到img标签里。

```htmlbars
<img src="图片地址" width=216 height=76 usemap="#rssmap" border="0">
<map name="rssmap" id="rssmap">
    <area shape=rect coords="0,0,103,16" href="/atom.xml" title="RSS订阅" target="_blank">
    <area shape=rect coords="113,0,216,16" href="http://www.google.com/ig/add?feedurl=/atom.xml" title="使用Google Reader订阅" target="_blank">
    <area shape=rect coords="0,20,103,36" href="http://9.douban.com/reader/subscribe?url=/atom.xml" title="使用豆瓣9点订阅" target="_blank">
    <area shape=rect coords="113,20,216,36" href="http://reader.youdao.com/b.do?url=/atom.xml" title="使用有道订阅" target="_blank">
    <area shape=rect coords="0,40,103,56" href="http://www.xianguo.com/subscribe.php?url=/atom.xml" title="使用鲜果订阅" target="_blank">
    <area shape=rect coords="113,40,216,56" href="http://www.zhuaxia.com/add_channel.php?url=/atom.xml" title="使用抓虾订阅" target="_blank">
    <area shape=rect coords="0,60,103,76" href="http://mail.qq.com/cgi-bin/feed?u=/atom.xml" title="使用QQ邮箱订阅" target="_blank">
    <area shape=rect coords="113,60,216,76" href="http://inezha.com/add?url=/atom.xml" title="使用哪吒订阅" target="_blank">
</map>
```

最后秀一下自己做的图片：

![](/uploads/2010/12/rss-template.png)
