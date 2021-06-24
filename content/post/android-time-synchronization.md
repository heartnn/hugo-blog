---
title: "[Android] 安卓时间校正"
date: 2013-01-05T21:38:00+08:00
lastmod: 2016-09-12
categories: 手机
tags: [android,ntp]
---

安卓系统本身包含有网络校时功能，但是由于基站、信号、移动网络等多种因素，校时不是很准确，所以heartnn都是在手机上用ClockSync来进行的，通过NTP服务器进行时间校正。

由于国内网络问题，不是所有的NTP服务器都可用，heartnn找来的下面的NTP服务器都来自网络，不保证一直可用，但发帖前都是测试可用的。

- time.asia.apple.com
- cn.pool.ntp.org
- asia.pool.ntp.org
- kr.pool.ntp.org
- time.pool.aliyun.com<!--more-->

附aClockSync中文版链接: https://pan.baidu.com/s/1dFIo101 密码: 2vtf
