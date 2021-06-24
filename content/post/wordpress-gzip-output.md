---
title: 关于Wordpress的gzip输出
date: 2010-10-29T01:51:00+08:00
categories: 技术
tags: [wordpress]
---

gzip输出的目的是可以有效地减少文件大小，以利于更快速的传输。

观看本文之前，请先到[这里](http://www.whatsmyip.org/http_compression/)检查你的网站是否已经启用gzip，如果已经启用，请忽略本文。

wordpress从2.5版本开始，不再提供gzip输出选项，而改代码又相对复杂，于是寻找一个可用的插件是必要的。

我用的是[wpCompressor](http://wordpress.org/extend/plugins/wpcompressor/)，目前的最新版本是0.3，插件很小，是单文件的。

使用中有个小问题，就是这个插件开启时，影响了xmlrpc，也就是说我的ScribeFire不起作用了。。。

于是乎改插件吧（本人不会做插件，但改改还是会的，`o(*≧▽≦)ツ）`，在gzip输出地时候排除了`xmlrpc.php`文件。反正这个文件也不会访问到的。 改好的代码在下面：<!--more-->

```php
<?php
/*
Plugin Name: wpCompressor
Plugin URI: http://playground.ebiene.de/16/plugin-fuer-gzip-komprimierung-der-beitraege-in-wordpress-25/
Description: wpCompressor automatic compression assumes the data output and boosts the performance of the blog pages.
Author: Sergej Müller
Version: 0.3
Author URI: http://www.wpSEO.org
*/

if (ob_get_length() === false && !ini_get("zlib.output_compression") && ini_get("output_handler") != "ob_gzhandler" && extension_loaded("zlib") && !headers_sent() && !is_admin() && stripos($_SERVER['REQUEST_URI'], 'wp-includes/js/tinymce') === false && stripos($_SERVER['REQUEST_URI'], 'xmlrpc.php') === false) {
    add_action(
    'init',
    create_function(
        '',
        '@ini_set("zlib.output_compression_level", 6); ob_start("ob_gzhandler");'
    )
    );
}
?>
```

另：由于空间转移时，heartnn发现[wpCompressor](http://wordpress.org/extend/plugins/wpcompressor/)不起作用了，于是又去wordpress.org搜索，发现了[GZIP Enable](http://wordpress.org/extend/plugins/gzip-enable/)和[Force gzip](http://wordpress.org/extend/plugins/force-gzip/)两个插件，都是用成功，而且在ScribeFile下也可以用，只是GZIP Enable下会有分类重复的问题(原来在wpCompressor貌似也有。)

这两个插件都是单文件的，其中GZIP Enable是base64加密的，所以不讨论其代码，Force gzip的代码夸张的长，并且包含了对浏览器是否支持gzip的判断，推荐使用。
