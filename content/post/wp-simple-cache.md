---
title: "简单实用的WordPress缓存插件——WP Simple Cache"
date: 2010-11-02T23:42:00+08:00
categories: 技术
tags: [wordpress]
---

![](/uploads/2010/11/wp-simple-cache.png)

自从前面[折腾了SourceForge]({{< relref "something-about-install-wordpress-on-sourceforge.md" >}})以后，总是不太死心，因为WP Super Cache总是安装不成功，我太懒，没有尝试手动安装，估计也很麻烦，光是要配置好那个config就需要改动好多地方，所以干脆去wordpress.org转了一圈，于是发现了[WP Simple Cache](http://wordpress.org/extend/plugins/wp-simple-cache/)。<!--more-->

下面是针对sourceforge的办法。还是用的persistent目录，这里简单说一下使用方法，假如你的wordpress在sourceforge的htdocs目录下，在persistent目录下建立wordpress目录，把原来应该上传在htdocs目录下的wp-content上传到persistent目录下的wordpress，比如我的目录结构现在是这样: `/home/persistent/h/he/heartnn/wordpress/wp-content`，如果你愿意的话，可以把`.htaccess`和`wp-config.php`也上传到wordpress目录里。

下面就比较简单了，把原来目录的相应文件改名，然后把新的做个软链接就行了。

```bash
cd /home/groups/h/he/heartnn/htdocs
ln -s /home/persistent/h/he/heartnn/wordpress/wp-content
ln -s /home/persistent/h/he/heartnn/wordpress/.htaccess
ln -s /home/persistent/h/he/heartnn/wordpress/wp-config.php
```

然后上传插件，以下几个目录或文件的权限设置为0777：

> /home/persistent/h/he/heartnn/wordpress/wp-content  
/home/persistent/h/he/heartnn/wordpress/wp-content/wp-simple-cache  
/home/persistent/h/he/heartnn/wordpress/wp-content/wp-simple-cache/cache  
/home/persistent/h/he/heartnn/wordpress/wp-config.php

最后就是去后台Enable WP Simple Cache，如果可以的话选上Compression会比较快一点(Sourceforge可能不行)，Show performance box是在页面的右上角显示WP Simple Cache的状态，为了自己debug用，开启后界面会不美观。

> 插件主页：<http://www.tankado.com/wp-simple-cache/>  
作者：[Özgür Koca](http://www.tankado.com/)

截止到2010年11月3日的最新版本是0.1.1。
