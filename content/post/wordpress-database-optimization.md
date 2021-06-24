---
title: WordPress数据库优化那点事
date: 2010-10-19T23:51:00+08:00
categories: 技术
tags: [wordpress]
---

由于最近又开始写博客了，于是重新折腾WP，但是毕竟两年不用，许多设置都生疏了，前两天更是因为一个插件问题，导致xmlrpc不能使用，更是郁闷了半天。

闲话少说，关于数据库优化来说，无非也就是那几点，先是版本控制，wordpress现在每修改一次文章，就会生成一个新的版本，着实让人不爽，对我们平头百姓也确实没什么用处。解决的方法很简单，用那个[Super Switch](http://wordpress.org/extend/plugins/super-switch/)插件吧，或者更简单的，在wp-config.php文件中加上

```php
define('WP_POST_REVISIONS',false);
```

一般我都是加在调试开关的下面。 接下来该处理的就是后台首页的rss内容了，网上的处理一般是在wp-config.php里增加<!--more-->

```php
define('MAGPIE_CACHE_ON', 0);
```

可是我在wordpress 3.0.1下测试的结果是失败。。。借助于[Clean Options](http://wordpress.org/extend/plugins/clean-options/)插件吗？貌似也不起作用，虽然此插件号称能删除包含rss_的内容，可是我却从来没搜索出来过。

通过自己的摸索，我编写了下面的SQL语句，可以再PHPMyAdmin下运行，也可以利用[WP-DBManager](http://wordpress.org/extend/plugins/wp-dbmanager/)搞定。

```sql
DELETE FROM `heartnn_db`.`wp_options` WHERE (`option_name` LIKE '%_transient_timeout_feed_%');
DELETE FROM `heartnn_db`.`wp_options` WHERE (`option_name` LIKE '%_transient_feed_%');
```

其中，`heartnn_db`是我的wordpress所在的数据库名称，请替换为您的博客所在的数据库。 经过这么一折腾，数据库轻松多了。目前还没有找到不修改源程序来禁止rss生成的其它办法，先这么凑合着了。
