---
title: "Ghost升级到0.9后感受"
date: 2016-08-03T13:07:26+08:00
categories: 日常
tags: [ghost]
---

首先说明的是升级后有一个[明显的bug](https://github.com/TryGhost/Ghost/issues/7168)，简单说就是静态页面在网址后加edit会404。

0.9最大的升级就是解决了服务器时间问题，现在可以选择时区了，算是一大进步。

Ghost渐渐抛弃了对nodejs 0.10的支持，转而使用LTS版本。

评论系统顺便切换到了多说，因为Disqus已经DNS污染，即便是https也不能访问到了，只能通过改hosts。
