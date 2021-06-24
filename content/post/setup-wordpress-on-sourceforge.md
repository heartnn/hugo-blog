---
title: "用SourceForge.net建立WordPress"
date: 2010-10-29T00:09:00+08:00
categories: 技术
tags: [wordpress]
---

![](/uploads/2010/10/about-sourceforge.png)

说来惭愧，6年前就创建了SourceForge(以下简称sf)的账号，但是从来没用过，最近无事，于是登陆来折腾一下～～发现sf空间的确是支持php的，很是欣喜。建立了一个Wordpress也成功了，当然用了点手段，想知道怎么折腾请往下看。(我不确定这样做是不是违反了sf的规定，因为sf本身提供了wordpress的博客，当然是不能自定义的那种了，下面会介绍到。)

首先是没有账号的先去注册个(这是废话。。。)，登陆后点上面的Create Project。

![](/uploads/2010/10/sourceforge-home.png)<!--more-->

来到了<https://sourceforge.net/register/>这个页面，点右边的Create Project，填写表单，很容易看懂的，注意的就是Url那里关系到以后的网址，不过也没关系，sf是提供域名绑定的，偷笑吧～～点Complete Registration完成。我是没点那个beta的链接，想尝试的可以试一下。

完成前面的步骤后点右上角你的用户名，在右边My Projects的框里应该能看到刚才创建的工程名字了。进入后再工程名下面的一排菜单里点Project Admin里的Feature Settings进行设置。

这里说点题外话，刚才提到sf提供了wordpress，其实就在Available Features里面，自己激活就可以用了，只不过域名要长的多，而且不能装自定义插件什么的。

![](/uploads/2010/10/sourceforge-feature-settings.png)

看上图，我们需要用到的三个功能，首先看网站管理，这个很简单，进入后必须设定一个密码才能使用，这个密码就是以后上传文件需要用到的。

关于数据上传，我用的是FlashFXP，链接类型要用SFTP over SSH，地址见下图，用户名的地方写用户名+英文逗号+工程名，其中工程名就是创建工程时url填写的那个，密码是刚才设定的密码。

![](/uploads/2010/10/sourceforge-sftp.png)

数据库管理比较麻烦，看下面的图就一目了然了。

![](/uploads/2010/10/sourceforge-mysql.png)

下面的框里分别设定只读用户，读写用户和管理员的密码，记得设定哦。

最后是绑定域名的方法：`yourdomain.com`与`www.yourdomain.com`是不同的，如果你想要`yourdomain.com`与`www.yourdomain.com`都能访问到，必须都要绑定。然后解析`yourdomain.com`的`A`记录到`216.34.181.97`，`www.yourdomain.com`的CNAME记录到`vhost.sourceforge.net`就行了，最多可以绑定10条记录。

然后开始wordpress的安装，都上传完以后，打开网址开始安装wordpress，此时必须手动建立一个`wp-config.php`，偷懒的话可以把`wp-config-simple.php`改名，然后把数据库的部分做修改（参考上面的图），需要特别注意的是MySQL主机名不是`localhost`，而是`mysql-h`(这里根据用户名不同而不同)。下面身份密匙设定的随机数可以参考<https://api.wordpress.org/secret-key/1.1/salt/>生成，其他的不用动了。

最后的注意点就是这个空间不能在wordpress目录生成`.htaccess`文件，所以我提供了我的`.htaccess`文件，大家手工上传一下就好了。

```apache
# BEGIN WordPress
<IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteBase /
    RewriteRule ^index\.php$ - [L]
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule . /index.php [L]
</IfModule>
# END WordPress
```
