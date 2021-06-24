---
title: 七牛关闭测试域名后的文件批量下载
date: 2018-10-09T08:05:34+08:00
categories: 技术
tags: [qiniu]
---

最近七牛关闭了所有域名的测试域名，没有备案域名的话，会造成bucket里的文件无法下载，反正我是没有备案域名，索性干脆全部导出来。

由于没有了默认域名，所以无法直接下载，不过由于新建的bucket提供30天测试域名，文件导出还是有办法的。

预备工作：首先下载七牛提供的[qshell](https://github.com/qiniu/qshell)，下载完毕后提取对应操作系统的可执行文件，比如windows系统重命名为qshell.exe。

```
qshell account AccessKey SecretKey
```

其中AccessKey和SecretKey去七牛后台可以找到，执行完毕后会在用户目录下生成`.qiniu`文件夹，里面的`account.json`记录了刚才的信息。

由于我保存文件的bucket的测试域名已经过期，这里在七牛控制台里新建一个名为temp的bucket用来作为中转，此处请保持新建的bucket和原有的在同一个区域(zone)。<!--more-->

此处假设我需要下载的bucket名为heartnn，那么首先将heartnn中的文件全部复制到temp。

一、先执行`qshell listbucket heartnn list.txt`将bucket生成文件列表，生成的文件可以作为csv处理，只保留文件名一栏即可。

二、然后执行`qshell batchcopy heartnn temp list.txt`，文件将全部复制到名为temp的bucket中。

三、之后新建一个config.conf，内容大致如下：

```json
{
"dest_dir" : "E:\\Users\\heartnn\\Desktop\\download",
"bucket" : "temp",
"access_key" : "xxxxxxxxxxxxxxxxxxxxxxxxxx_xxxxxxxxxxxxx",
"secret_key" : "xxxxxxxxxx-xxxxxxxxxxxxxxxxx-xxxxxxxxxxx",
"cdn_domain" : "pgb0ubhq8.bkt.clouddn.com",
"is_private" : false,
"prefix" : "",
"suffix" : ""
}
```

四、最后执行`qshell qdownload 3 config.conf`进行下载，其中3为线程数。

如果有多个bucket需要下载，可以先执行`qshell batchdelete temp list.txt`将temp清空，然后再复制新的bucket进去。
