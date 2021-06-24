---
title: "还你一个纯净的WPS 2019"
date: 2018-12-05T17:44:57+08:00
categories: 软件
tags: [wps]
---

本方法来自网络并根据新版有所更改。

近几天，电脑上的微软Office似乎出了问题，保存的文件直接坏掉了，不想纠结太多，也不想重装系统。恰逢WPS 2019界面看起来不错，所以开始折腾一番。

以11.1.0.8002为例，正常安装到默认目录`C:\Users\heartnn\AppData\Local\kingsoft\WPS Office`，以下说明以此目录为标准，大家可以根据自己的安装路径调整。

1\. 去掉广告，在主界面的设置里找到配置和修复工具，点高级-->其他选项，可以把软件推荐和广告推送都关掉，顺便从升级设置里禁用升级。

![](/uploads/2018/12/wps-ad-push.png)<!--more-->

2\. 管理工具-->服务中禁用WPS Cloud服务。

![](/uploads/2018/12/wps-cloud-service.png)

3\. 计划任务中有两条与WPS相关的，删之。

![](/uploads/2018/12/wps-cronjob.png)

4\. 托盘里关掉WPS云的右键以及在我的电脑中显示。

![](/uploads/2018/12/wps-tray.png)

![](/uploads/2018/12/wps-tray-settings-my-computer.png)

![](/uploads/2018/12/wps-tray-settings-rightclick.png)

5\. 用零字节文件覆盖以下文件：

- 安装目录下的`ksolaunch.exe`、`wps.exe`、`wpscloudsvr.exe`。
- `11.1.0.8002`目录下的`khomepage.dll`、`ksolaunch.exe`、`wpscenter.exe`、`wpscloudlaunch.exe`、`wpscloudsvr.exe、wpscloudsvrimp.dll`。
- `wtoolex`目录下的`updateself.exe`、`wpsnotify.exe`、`wpsupdate.exe`。

经过以上步骤，就得到一个完美的本地版WPS 2019了，因为覆盖掉了`ksolaunch.exe`，所以不可以登录WPS云。

### 参考

- <https://www.cnblogs.com/onelikeone/p/9276349.html>
- <https://blog.csdn.net/checkerror2/article/details/79515517>
- <https://blog.csdn.net/cn_dxtz/article/details/79415565>

- 20181212: 添加图片说明。
