---
title: 从 115 网盘到 TheWorld 的用户配置
date: 2010-12-06T06:22:00+08:00
categories: 日常
tags: [browser,php]
---

本来是不相关的事情，最近联系到了一起。起源就是一段 115 网盘外链的 php 代码，说是外链，实际上算是盗链的了，所以 heartnn 也没敢用，只是在这里贴出来吧。

```php
<?php
/*
 * (C) Copyright 2009-2010 115.com All Rights Reserved
 *
 * 115 网盘外链 php 版
 * 空间需要支持 allow_url_fopen
 * 外链形式：http://115.pp.ru/115.php/提取码/xxx
 * 2010.11.14 亲测有效
 * 作者：haowenq
 * 博客地址：http://rr.org.ru
 *
 */
    $uri = $_SERVER["REQUEST_URI"];
    preg_match("/115.php\/(.+)\//",$uri,$code); //自己修改
    $code = $code[1];
    $opts = array(
        'http'=>array('method'=>"GET",'header'=>"User-Agent: Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.3)\r\n")
    ); //伪造 User-Agent
    $context = stream_context_create($opts);
    $url = "https://uapi.115.com/?ct=upload_api&ac=get_pick_code_info&pickcode=".$code."&version=1164"; //获得原始下载地址
    $data = file_get_contents($url,false,$context);
    $data = str_replace("\\","",$data);
    preg_match_all("/\"Url\":\"(.*?)\"/", $data, $data);
    $myurl = $data[1][2]; //获得备份下载
    if($myurl){
        header('Content-Type:application/force-download'); //强制下载
        header("Location:".$myurl);
        die();
    }
    else echo "提取码不存在或已过期";
?>
```

从 115 的网站上看到了 115 出的浏览器，软件不大，只有几百 KB，心想肯定是 IE 核心的，又一想最近在调试网站，何不下载一个代替 IE 呢。（heartnn 后来证明 IE 核心的浏览器和 IE 完全不是一回事的，证据是 heartnn 在本地调试一段 css 代码的时候，所得到的结果是不同的，尤其是在字体大小方面。）

软件不大，而且是纯绿的，在用了一天多的时间以后，发现一个重大问题，就是打开上次浏览的标签问题，115 根本就不打开上次未关闭的标签，甚至在选项内设置了也无用。

好在 115 和 Theworld 很相似（现在这篇文章的标题算是连上了。。。），Theworld 是 heartnn 再熟悉不过的浏览器了，从 1.x 就开始使用。下载绿色版的 3.x 后解压缩到桌面上(桌面在 D 盘里)，此时诡异的事情出现了，当前目录里并没有生成 ini 配置文件及相关的 data，关闭后再打开的话配置倒是不会丢失，难道是保存到系统用户目录去了？用 Everything 搜索了一下，原来配置果然是保存在 `%appdata%` 里面了。于是去官方论坛，有人说 Win7 下就是保存到 `%appdata%` 下的，有的则说没有问题，难道是我人品问题吗？

于是不断地搜索，一开始搜到的大多是配置无法保存的问题，或是配置界面出错的问题，怎么就没人和我的问题一样吗？后来看见一个帖子里说“是不是把Theworld放到系统盘里了”，才恍然大悟，原来 heartnn 的系统虽然在 C 盘里，但是用户配置啥的都是在 D 盘里的，难怪Theworld会把配置放到`%appdata%`里了，这也应该算是软件的 bug 了吧。

PS. 那时郁闷，在饭否发了条消息说自己在折腾浏览器，结果 Opera China 回复了，自己大 `⊙﹏⊙b` 汗，折腾 IE 核心，说出去多丢人啊。。。

2010-12-09 注：没想到那段盗链代码这么快就失效了。。。
