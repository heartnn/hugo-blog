---
title: "在OpenShift安装Tiny Tiny RSS的注意事项"
date: 2015-04-21T00:34:00+08:00
categories: 技术
tags: [php,rss,openshift]
---

上个月心血来潮在OpenShift上重新搭建了一个Tiny Tiny RSS，原来的用的是OpenShift提供的快速安装，数据库也是PostgreSQL，程序也比较旧了，干脆删掉重来。

先挂上一个运行了一个月的截图：

![](/uploads/2015/04/tt-rss.png)

简单叙述一下安装过程，此过程需要一定的git以及OpenShift使用经验。<!--more-->

首先是下载源代码，由于官网无法访问，可以从[github](https://github.com/gothfox/Tiny-Tiny-RSS/releases)上下载，或者直接

```bash
git clone https://github.com/gothfox/Tiny-Tiny-RSS.git
```

接下来修改配置文件，下面列出部分的配置文件，这样写可以不用费力的填写用户名密码等等：

```php
define('DB_TYPE', "mysql");
define('DB_HOST', getenv('OPENSHIFT_MYSQL_DB_HOST'));
define('DB_USER', getenv('OPENSHIFT_MYSQL_DB_USERNAME'));
define('DB_NAME', getenv('OPENSHIFT_APP_NAME'));
define('DB_PASS', getenv('OPENSHIFT_MYSQL_DB_PASSWORD'));
define('DB_PORT', getenv('OPENSHIFT_MYSQL_DB_PORT'));
define('MYSQL_CHARSET', 'UTF8');
define('SELF_URL_PATH', 'http://'.getenv('OPENSHIFT_APP_DNS'));
```

接下来是feed的自动更新问题了，先说几句题外话，在带有cpanel的主机空间里可以通过创建cronjob来完成，可以参考[官方的安装说明](http://tt-rss.org/wiki/InstallationNotes)。

在OpenShift中，首先需要在项目中添加Cron 1.4，点下面的see the list of cartridges you can add，找到Cron 1.4，然后添加到项目中。

![](/uploads/2015/04/openshift-add-cartridge.png)

然后编辑定时执行的脚本(比如名称为`fetch-feeds`)，放到本地项目`/.openshift/cron/hourly`，可以达到每小时更新(minutely对应每分钟fetch feed)

```bash
#!/bin/sh

$OPENSHIFT_REPO_DIR/update.php --feeds --quiet &> $OPENSHIFT_LOG_DIR/rss_update.log
exit 0
```

将要安装到的OpenShift项目clone到本地，然后添加tt-rss的代码，再通过Git将项目提交到OpenShift。

然后修改下列项目的权限：

| 目录 | 权限 |
| --- | --- |
| /cache及其包含目录 | 777 |
| /feed-icons | 777 |
| /lock | 777 |
| config.php | 777 |
| /.openshift/cron/hourly/fetch-feeds | 755 |
| /update.php | 755 |

最后运行<https://xxx-xxx.rhcloud.com/install>完成安装。
