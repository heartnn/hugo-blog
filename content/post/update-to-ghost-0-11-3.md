---
title: "Ghost变得越来越成熟了，已跟随更新到0.11.3"
date: 2016-11-27T14:00:29+08:00
categories: 日常
tags: [ghost]
---

因为新版Ghost已经不支持node 0.10，建在Docker上的ghost可以切换版本最新到6.9，在Openshift上可以利用[@icflorescu/openshift-cartridge-nodejs](https://github.com/icflorescu/openshift-cartridge-nodejs)另外建一个自动更新版本的node。

这里比较注意的是需要通过`.openshift/NODE_VERSION_URL`和`.openshift/NPM_VERSION_URL`来自定义版本，最好是使用node自带的npm版本(更新到最新的npm貌似不行。)

```plain
https://semver.io/node/resolve/6.9.1
https://semver.io/npm/resolve/3.10.8
```

剩下的直接把本地安装好的Ghost通过git上传到根目录即可，可以抛弃原来的quickstart了。
