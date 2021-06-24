---
title: 关于Ghost下的301跳转
date: 2015-12-08T02:51:05+08:00
lastmod: 2016-03-03T11:39:12+08:00
categories: 技术
tags: [openshift,ghost,301]
---

借着升级0.7.2，一起解决了non-www向www的域名跳转问题，本来是个很简单的问题，就因为Openshift的特殊性，变的复杂起来。

如果Google这个问题的话，会有[www向non-www跳转的解决办法](http://codenimbus.com/2014/01/15/redirecting-www-domain-to-non-www-on-ghost/)，并且似乎需要修改`core/server/routes/frontend.js`，而且容易引起重定向循环。

如果用nginx反向代理的话应该会很简答，用Apache的话也可以用`.htaccess`解决，但是Openshift里的Nodejs自带的Apache似乎没有mod_rewrite。

所以结论是在Openshift新建一个php 5.4，利用一下.htaccess。<!--more-->

```apache
<IfModule mod_rewrite.c>
Options +FollowSymlinks
RewriteEngine on
RewriteCond %{HTTP_HOST} ^heartnn.com
RewriteRule ^(.*)$ http://www.heartnn.com/$1 [R=301,L]
</IfModule>
```

RewriteRule直接用的域名而不是%{HTTP_HOST}也是怕引起重定向循环，这空间还真是特殊啊。

20160303: 今天发现Cloudflare通过Page Rules可以搞定一切，之前都算白折腾了。关于设置可以参考下面的图片，另外两个域名指向一起就行了。

![](/uploads/2015/12/cloudflare-page-rules.png)
