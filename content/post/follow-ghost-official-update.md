---
title: 紧随Ghost的官方更新
date: 2016-02-20T21:43:49+08:00
categories: 日常
tags: [ghost]
---

最近[Ghost更新](https://github.com/TryGhost/Ghost/releases)的有些频繁，0.7.7仅仅一天就被更新，每次`npm instsall --production`都要漫长的时间。最好的办法就是换源：

```dos
npm config set registry https://registry.npm.taobao.org
```

有一点疑惑，就是本地`npm install --production`时，会卡在`preinstall`这里，也就是`npm install semver`之后`startup-check.js`过不去，跳过也不影响用，heartnn安装的node版本是0.10.35，和Openshift是一样的。<!--more-->

为了避免各种麻烦，我已经将完整的包放到[github](https://github.com/heartnn/openshift-ghost-quickstart)上。当Openshift全新安装时，直接在Openshift后台建立一个node.js程序，然后在建立时导入上面的仓库，速度最快；如果需要更新旧版本，可以直接clone到本地修改。

> 关于Windows复制时提示路径太长的问题，可以先用Winrar打包，然后解压缩到目标目录。

如果是非Openshift空间，请转到`node_modules/ghost`目录内，这里是集成了Production环境的完整包。
