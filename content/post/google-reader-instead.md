---
title: "Google Reader 替代方案"
date: 2013-04-24T08:08:00+08:00
categories: 网络
tags: [php,rss]
---

距离Goolge宣布停止[Google Reader](https://www.google.com/reader/)已经有一个多月了，期间heartnn换了很多RSS Reader，但是都没什么好用的，或者说都不太完美。

先说[feedly](https://feedly.com/)，是大家炒的最火的，先不说服务器间歇被墙，和Google Reader的样子也有一定的差距，不过digest模式很新鲜。

[TheOldReader](http://theoldreader.com/)是我比较推荐的，而且最近有中文语言了，但是界面友好性好像差一点，创建目录很繁琐，而且在旧电脑上网页反应很慢。最重要的是承诺的Android客户端貌似到现在也没有。

国内的RSS阅读器都有一个最重要的问题，就是不能访问某些RSS地址(具体原因你懂的)，客户端就更别指望了，都是一塌糊涂，在手机上访问很不友好，这里面比较好的是[鲜果阅读器](http://xianguo.com/reader/)(注意不是鲜果首页)。<!--more-->

[NewsBlur](http://newsblur.com/)是个很强大的东西，可惜heartnn不会建啊，默认只能添加40多个feeds，如果feed很少的话可以考虑，或者可以自己动手的话去git它的源码。

以上就是网络上大致的RSS Reader了，如果都不满意的话，而且手头有PHP空间的话，可以继续往下看。

先介绍一个[selfoss](http://selfoss.aditu.de/)，单用户RSS Reader，界面很友好，可以到官网看看截图，而且提供手机界面，可以完全public，或者加入用户名密码来私人访问。推荐★★★★，安装难度★★★。安装需要配置config.ini，有点小难度，需要注意的是点击文章自动标记为已读需要从defaults.ini里修改。服务器更新期间有点小占资源，需要添加计划任务才能update。

[Tiny Tiny Rss](http://tt-rss.org/)是目前我尝试的最好用的，而且还有Android客户端哦，安装配置也很简单，基本不用什么设置就可以使用，如果使用Android客户端的话要到用户设置里打开api才可以，Ajax载入速度尚可，可以支持[PubSubHubbub协议](http://pubsubhubbub.googlecode.com/)（但是大家一般都没有吧，需要的话可以[到这里看看](https://code.google.com/p/pubsubhubbub/wiki/Hubs)）。最重要的是可以打开注册功能，多人共享，支持share列表（类似Google Reader的Share功能），加星等等，含有插件功能，可以把想读的内容放进[Pocket](http://getpocket.com/)里。

网上有几个提供了Tiny Tiny RSS注册的地方，先列出到这里：

 - <http://news.parlementum.net/>
 - <http://reader.andreafortuna.org/>
 - <http://www.feedsreader.net/>
 - <https://rss.uugrn.org/>（这个应该不错，提供SSL连接的）

另外还有一个[Lilina](http://getlilina.org/)，heartnn没有尝试，界面是仿Mac的样子，大家可以去官网围观。

20130530: 发现[LinuxTOY](http://linuxtoy.org/archives/inoreader.html)介绍了一款在线阅读器，名字叫做[InoReader](https://www.inoreader.com/)，试用了一下很不错，可以完整导入Google Reader，连原来收藏的文章都可以导入。

20130622: [Wow!Ubuntu](http://wowubuntu.com/slickreader.html)介绍了一款基于Newsblur的[SlickReader](https://slickreader.com/)，免费账户可以不限制feed数量，但是heartnn试用了下，貌似个别国内的feed会无法解析到。
