---
title: TiddlyWiki、DokuWiki、PmWiki——三款简洁的Wiki软件对比
date: 2011-09-15T02:42:00+08:00
categories: 软件
tags: [php,wiki]
---

近些天在研究几款Wiki软件，为了记录一些笔记或者网上摘抄的文章，需要富媒体的，所以抛弃了许多的笔记软件，因为用了网络同步的关系，又不能使文件过大，所以筛选了一段时间后，剩下了三款。

三款软件分别为[TiddlyWiki](http://www.tiddlywiki.com/)、[DokuWiki](http://dokuwiki.org/)、[PmWiki](http://www.pmwiki.org/)，其中TiddlyWiki是html+javascript的，最为简洁，而且是单文件，这个网上一搜的话一大把，另外两款都是需要php支持的，但是不需要数据库的支持。

本人推荐的顺序是：DokuWiki > TiddlyWiki > PmWiki，值得一提的是，目前这三款软件都是支持中文的，但是插件什么的中文资源却很少。

对比三款软件的功能(在不安装任何插件的情况下)，由于本人是利用网盘同步的，所以首先是文件大小的问题，Doku每个wiki条目会生成单独的txt文本，而且每次编辑都会有版本文件生成，虽然被压缩了，但是同步的时候还是会进行的。

然后是PmWiki，文件版本会集成在每个wiki文件内，这样不会有版本文件产生，但是当有条目被删除的时候，会生成一个delete文件。<!--more-->

相比前两款php下的软件，tiddly可以说是简单的不行，始终是那一个文件，只是体积在不断增大，如果条目很多的话，这个文件到2mb多也是有可能的。

所以如果大家和我一样是用网盘同步的话，三款软件是各有优劣的。

再说说三款软件的便携性，Tiddly是最方便的，只需要浏览器就可以了，IE下最方便，Firefox和Opera下需要TiddlySaver.jar的，并且需要浏览器启用Java，也就是说电脑里装JRE才可以在Firefox和Opera下保存(另外Chrome下貌似很麻烦)，关于保存问题，可以参照<http://www.tiddlywiki.com/#SaveChanges>。

另外两款软件如果想便携的话，就需要一个php开发环境，其实不需要那么复杂，有一款叫[QuickPHP](http://www.zachsaw.com/?pg=quickphp_php_tester_debugger)的软件，很小，文件数量也很少，便于携带，是基于Windows的。 以后将逐步介绍三款软件的汉化方法以及功能等等。
