---
title: 360WiFi在Win10下的兼容测试
date: 2015-11-26T06:48:21+08:00
categories: 软件
tags: [360,wifi]
---

![](/uploads/2015/11/360wifi3.jpg)

先来说说结论，用的[11月25日最新驱动](http://shequ.mall.360.com/forum.php?mod=viewthread&tid=6181175)，号称是可以支持大部分Win10的，然并卵。。。

具体状态表现为可以创建WiFi并正常显示，但使用手机连接时卡在“正在获取IP地址”的步骤。<!--more-->

heartnn用的是[无约而来的Win10 TH2 X64企业版](http://zxkh19501.blog.163.com/blog/static/123785179201510179525522/)，一开始的确是ESET的问题，卸载后怀疑和Windows Firewall冲突，但关掉以后还是不行。

最后只能将360AP.exe的兼容性改为Win8并更改所有用户的设置(我是启用了Win10的内置管理员帐户)，和Windows Firewall没有任何关系。

最后不得不说，像小度用着没有任何的问题，可见360的开发细节仍需注意。
