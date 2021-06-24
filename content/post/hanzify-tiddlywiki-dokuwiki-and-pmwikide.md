---
title: "TiddlyWiki、DokuWiki、PmWiki的简单入门: 汉化"
date: 2011-09-18T08:52:00+08:00
categories: 软件
tags: [php,wiki]
---

[之前的文章]({{< relref "compare-tiddlywiki-dokuwiki-and-pmwiki.md" >}})对三款Wiki软件进行了简单的对比，接下来是更详细的功能对比。

首先大家最关心的应该是中文语言吧，这三款软件都能做到中文化，只是操作起来的简易程度不一样，个人认为最简单的应该是DokuWiki了，直接从[官方下载页面](http://www.splitbrain.org/projects/dokuwiki)下载stable版本就可以了，内含中文语言包，只需要安装配置的时候选简体中文就可以了。

关于TiddlyWiki的汉化，网上其实也有很多资源，需要入门的话可以去[TiddlyWiki華語邦](https://sites.google.com/site/tiddlywikizh/)，那里有完全指南，也有汉化的方法。这里简单说一下汉化方法吧，先[下载汉化文件](http://svn.tiddlywiki.org/Trunk/association/locales/core/zh-Hans/locale.zh-Hans.js)，然后在TiddlyWiki里新建一个文档，名字就叫zh-HansTranslationPlugin吧，把刚才汉化文件里的文本复制进去，在tag里填上systemConfig，这个标签是最重要的，有了这个标签，这篇文章才会被系统识别成插件。然后就刷新TiddlyWiki就可以了～～<!--more-->

至于PmWiki的汉化，个人认为是最麻烦的，虽然PmWiki有[i18n](http://pmwiki.org/pub/pmwiki/i18n/)，但是还要手动在配置文件中添加设置才可以用。先把所有i18n文件都覆盖过来，然后编辑local/config.php文件，找到

```php
include_once("scripts/xlpage-utf-8.php");
```

去掉这一行的注释，然后在最下面添加

```php
XLPage('zhcn','PmWikiZhCn.XLPage');
```

然后进入首页，菜单神马的都变英文了，如果想进入中文首页，就要在n=后面输入`PmWikiZhCn.PmWikiZhCn`才可以。关于更多复杂的设置，以后可能会单独写一篇博客来说明，毕竟不是一两句能说清楚的。
