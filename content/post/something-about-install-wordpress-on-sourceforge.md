---
title: 关于Sourceforge上安装WordPress的二三事
date: 2010-11-02T01:35:00+08:00
categories: 技术
tags: [wordpress]
---

前面写过[如何在Sourceforge上安装wordpress]({{< relref "setup-wordpress-on-sourceforge.md" >}})，但是经过后来的调试，不像我想象的那么简单，因为这个空间的写入问题，所以很多插件都不能使用，下面说说感受。

首先是cache类插件不用想了，由于wp-content目录不可写（可是我明明设置成777了的），所以cache是无法生成的，启动[WP Super Cache](http://wordpress.org/extend/plugins/wp-super-cache/)的后果就是无法进入后台管理。。。空间速度其实还是不错的，尤其是从国外访问，所以没有cache就没有了吧。(这里我想了又想，很可能是服务器的缘故，不单纯是文件夹权限问题。)

写入wp-config.php的插件也不行，比如[PS WP Multi Domain](http://wordpress.org/extend/plugins/ps-wp-multi-domain/)，不过可以自己编辑一下。类似的，写入.htaccess的也必须手动编辑。<!--more-->

目前问题最多的地方就是目录权限问题，其次是无法访问外部服务器，很明显的就是后台首页中的feed都无法更新，Akismet无法使用，对于Akismet有个替换的方案，就是安装[Math Comment Spam Protection](http://wordpress.org/extend/plugins/math-comment-spam-protection/)(昨天晚上测试了很多垃圾屏蔽插件，就这个工作的最好，而且前台评论时还显示中文的)，看名字就知道，是通过算术来防止垃圾评论的。

另外，网上有说[需要设置权限的文件夹放在persistent目录里]({{< relref "wp-simple-cache.md" >}})，然后再用软链接过去，但我的操作是失败的。
