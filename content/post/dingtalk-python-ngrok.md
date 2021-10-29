---
title: "自编译基于Python的钉钉内网穿透(ngrok)"
date: 2021-10-29T15:32:16+08:00
categories: 软件
tags: [python,nat]
---

源代码来自于[hauntek/python-ngrok](https://github.com/hauntek/python-ngrok)，自己又从`dingtalk/ngrok`中提取了源码进行比对，然后用pyinstaller编译而来。

用起来还是相当稳定的，虽然似乎只能穿透http，但是能穿透一个群晖的DSM就足够了，建议应急的时候用一下。

下载地址：<https://github.com/heartnn/dingtalk-ngrok/releases/latest>，包含Windows和Linux的版本。
