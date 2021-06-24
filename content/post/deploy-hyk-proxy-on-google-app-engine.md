---
title: 在GAE上部署hyk-proxy
date: 2010-09-19T01:45:00+08:00
categories: 技术
tags: [java,proxy]
---

hyk-proxy是一个web proxy框架，支持基于[Google AppEngine](https://appengine.google.com/)平台和[Seattle平台](https://seattlegeni.cs.washington.edu/)，以及PHP Web空间的proxy实现。

在这篇文章中，我们只介绍最简单的Google AppEngine下的搭建及客户端的配置。

### 准备工作

 - 基础环境：[JRE](http://www.java.com)
 - 源代码：[hyk-proxy-gae-server-0.9.0.zip](https://code.google.com/p/hyk-proxy/downloads/detail?name=hyk-proxy-gae-server-0.9.0.zip&amp;can=2&amp;q=) 和 [hyk-proxy-0.9.0.zip](https://code.google.com/p/hyk-proxy/downloads/detail?name=hyk-proxy-0.9.0.zip&amp;can=2&amp;q=)
 - GAE Java SDK：[appengine-java-sdk-1.3.7.zip](https://googleappengine.googlecode.com/files/appengine-java-sdk-1.3.7.zip)<!--more-->

### 一

首先安装JRE，或者你的机器中有JDK也可以，这保证了java程序能够运行。熟悉GAE程序Deploy的同学可以跳过下面的步骤，直接用appcfg.cmd上传(记得改App ID哦)。

将hyk-proxy的源代码和GAE java SDK解压缩，运行server中的install.bat(Linux下请运行install.sh)，如果你是第一次运行AppCfgWrapper的话，会出现下面的对话框：

![](/uploads/2010/09/appcfgwrapper.png)

这时，要把SDK Location的位置选择在你[appengine-java-sdk-1.3.7.zip](https://googleappengine.googlecode.com/files/appengine-java-sdk-1.3.7.zip)解压缩的位置，然后点Apply。

回到主界面后，appID选择你申请的xxxx.appspot.com中的xxxx部分，AppLocation选择hyk-proxy-gae-server-0.9.0\war目录，然后填上邮箱和密码，点Deploy就可以了。到此上传步骤完成。

### 二

现在打开`hyk-proxy-0.9.0\bin`目录，运行`startgui.bat`，可以看到客户端的主界面，点Plugins，会看到已经安装的插件，点下面的GAE 0.9.0，再点Config可以配置插件。

![](/uploads/2010/09/hyk-proxy-plugins.png)

按New新建一个proxy服务器，AppID写刚才部署的服务器名，一路OK下去就可以了，这样的话会用匿名账号访问代理服务器进行中转，至此，客户端状态应该是这样的。

![](/uploads/2010/09/hyk-proxy.png)

主界面下的Config界面中可以设置启动时是否自动连接。

从浏览器中打开`http://xxxx.appspot.com`，其中xxxx是你刚才部署的地址，会出现**"hyk-proxy-gae 0.9.0 server is running!"**的字样，就说明成功了。

把浏览器的代理设置为`127.0.0.1:48100`就可以通过该代理进行穿越访问了。如果你觉得这就够用了的话，那么下面的都可以忽略。

### 三

关键部分：流量限制以及使用者限制。

回到刚才打开的xxx.appspot.com页面上，点admin，使用GAE的邮箱密码进行登录，会出现该程序的root密码。

![](/uploads/2010/09/webroot.jpg)

接下来打开`hyk-proxy-0.9.0\bin\admin.bat`，选择`[2] GAE V0.9.0`，appid填入你部署的地址，login填`root`，密码填刚才看到的密码。直到出现$提示符则成功。

接下来限制用户吧，用users命令列出当前存在的用户，有anonymouse和root两个，使用`passwd 用户名`可以修改当前用户的密码。

使用`useradd`命令添加新用户，值得提醒的是新用户的名称是用邮箱名称的，而且会发信到目标邮箱里。

关于流量控制，可以用类似下面的命令：

```dos
traffic -u anonymouse -s * -r 1073741824
```

其中：

 - `-u` 流量限制的用户名。
 - `-s` 限制流量的网站名，用*代表全部网站，如果只限制某一网站，填写域名即可。
 - `-r` 限制的流量。

上面例子中是100MB 至此，一个hyk-proxy算是完成了，虽然还没有使用到完全的功能，但是一般的代理功能已经实现了，而且https的访问也算正常（除了证书不正确以外，这是代理的通病，作者说用[Seattle](https://seattlegeni.cs.washington.edu/)可以解决。），登陆twitter也没有问题。 关于hyk-proxy的其它问题，有时间了再写吧。
