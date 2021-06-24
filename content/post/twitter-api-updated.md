---
title: "Twitter API终于升级了"
date: 2013-06-13T22:15:00+08:00
categories: 网络
tags: [twitter]
---

头两天就发现自己手机上的Twicca不能刷推了，于是网络一搜，发现终于强制升级到API 1.1了，之前就听说，但是一直没关注日期。

自己的Twidere不受影响，因为用的是GTAP，而且一直用的1.1接口，自用的Twip就比较麻烦了，必须git作者的最新版（这其中自己偷懒，用了原来的config.php，发现有点不一样），完整的重新配置才可以。

而且Twicca也必须更新到最新版(目前是0.C.0)，旧版已经不支持API 1.1了，修改API版看[这里](http://zhyu.me/android/twicca-mod.html)，感谢@zhyu。

万恶的Twitter对API限制很严重，目前我用的是Hootsuite的一组key，这样的话可以没那么严格的控制。<!--more-->

下面是Hootsuite的key：

> key: w1Gybt9LP9zG46mS1X3UAw  
secret: hRIK4RWjAO4pokQCvmNCynRAY8Jc8edV1kcV2go6g

[菜牛的天空](http://b.xiacd.com/2013/05/twitter-api-v1-1-oauth-key/)发了好几组key，heartnn没有一一测试，但是请注意那些没有callback url的是不能在Twip上用的，会出现401错误（上面Hootsuite的那个不会）。

2013-06-15补充: Twicca中头像部分貌似不会通过api进行下载，需要挂vpn等下载，或者改hosts，个人倾向于改hosts，比较方便。对应域名包括`a0.twimg.com`和`a1.twimg.com`(可能会有更多)。

2013-09-11: 最近发现用Hootsuite会发现不定期登陆失效的问题，需要重新登陆，这样的话在手机上用很不方便，所以建议大家还是用自己的API吧，但貌似会有每小时使用次数限制。
