---
title: Openshift安装Ghost的注意事项
date: 2015-06-11T22:12:28+08:00
categories: 技术
tags: [openshift,ghost,cloudflare]
---

首先是Openshift的二级域名，一定要ping一下对应的IP，如果是通的，那么意味着绑定域名后是可以正常访问的，如果不通的话，则域名必须绑定到Cloudflare这样类似可以提供CDN的域名解析，或者用Cloudflare提供的SSL访问。

Openshift提供了[快速部署](https://github.com/openshift-quickstart/openshift-ghost-quickstart)，建议使用，如果一定要自己搭建，需要Google补充许多知识，费力费时，得不偿失。Openshift快速部署目前提供的是0.5.10的版本，和目前的最新版没有太大的区别。

建议使用git方式部署Ghost，方便以后修改模板等。ssh上传速度可能不快，而且以后代码迁移也不如git方便。不过如果图片附件等不使用第三方存储时建议采用此方法。<!--more-->

Ghost只提供图片上传，其他类型附件目前只能自己手动上传，当采用git方式部署时，不要使用ghost自带的存储目录，而是使用第三方存储([ghostchina](http://www.ghostchina.com/)提供了七牛云、又拍云和阿里云等方案，heroku提供了存储在Amazon S3的方案)。

关于程序升级，如果使用的是快速部署，请先clone到本地，然后用新版Ghost文件覆盖到`/node_modules/ghost/`，覆盖前注意备份自定义的主题等等，然后复制一份新的主题到`/content/themes`，修改`/package.json`文件dependencies下ghost的版本号。

最后写一下有关绑定到Cloudflare的注意事项，如果博客内容中没有引用http资源的话(尤其是视频)，强烈建议将博客改为https访问，无论如何也是相对安全的，可以按下图配置Crypto：

![](/uploads/2015/06/cloudflare-crypto.png)

具体在使用Cloudflare时，注意关闭Speed标签下Rocket Loader功能，否则可能引起Ghost后台不正常。
