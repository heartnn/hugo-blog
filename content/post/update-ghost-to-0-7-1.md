---
title: "升级Ghost到0.7.1"
date: 2015-10-08T22:31:35+08:00
image: "/uploads/2015/10/ghost.png"
categories: 日常
tags: [ghost]
---

[Ghost](https://github.com/TryGhost/Ghost/releases)的升级并不像Wordpress那么方便，但也不会像网上说的那么复杂，最近成功从0.6.3的中文版升级到了0.7.1原版，简单记录下心得。

本人使用的是非正常的升级方法，本地也没有安装nodejs和npm什么的，利用了[ghostchina的完整包](http://www.ghostchina.com/download/)，将node_modules复制，省掉了`npm install --production`的步骤(ghostchina上也有介绍)。<!--more-->

然后对比旧版的config.js修改新版的，并对新旧模板比较修改，对于官方版，需要对default.hbs中的Google字体进行修改。将

```htmlbars
<link rel="stylesheet" type="text/css" href="//fonts.googleapis.com/css?family=Merriweather:300,700,700italic,300italic|Open+Sans:700,400" />
```

修改为

```htmlbars
<link rel="stylesheet" type="text/css" href="{{asset "css/fonts.css"}}" />
```

下载[原版的Google字体](https://fonts.googleapis.com/css?family=Merriweather:300,700,700italic,300italic|Open+Sans:700,400)，重命名为`fonts.css`，放到`assets/css`中，下载其中所有的woff2字体，放到`assets/fonts`中，并将`fonts.css`中所有字体链接地址改为`../fonts/字体名称`。

最后可以重启nodejs看看效果了，如果是从ghostchina的中文版升级到原版，可能需要先进入后台修改默认模板(这里如果不能生效的话可能需要修改数据库)，或者重命名模板，并修改模板中的`package.json`。
