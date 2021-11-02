---
title: "将TM-AC1900刷回RT-AC68U"
date: 2019-03-23T07:32:17+08:00
category: 技术
tags: [router]
---

之前由于入手了树莓派，就给原来的[华硕RT-AC68U]({{< relref "start-merlin-from-zero-with-syncthing-installed.md" >}})改成了官方固件，这两天手贱升了一下级，于是路由器被打回了原型，固件变成了3.0.0.4.376_3199(T-Mobile定制版，功能较少)，从网上查了一下，似乎刷旧版固件然后取得ssh的方式不能用了，虽然用起来区别不大，但总觉得不甘心，于是折腾模式开启。

### 准备工作

1\. cfe，最好是自己以前备份过的，实在没有可以自己生成一个，我的就是没有备份过，所以只能自己生成了。

首先下载我提供的[CFE模板](/uploads/2019/03/cfe-1.0.2.0-all-am.7z)，然后参照[截图](/uploads/2019/03/cfeedit.png)用[CFEEdit](/uploads/2019/03/cfeedit.7z)修改里面的MAC地址和WPS密码。

本机的MAC地址可以登录<http://cellspot.router/>,然后点WLAN可以查看到，LAN和2.4G的MAC地址似乎是一样的。WPS密码可以参照路由器背面。

然后打开网站<https://cfeditor.pipeline.sh/>，把修改好的CFE上传到Original CFE，Source CFE选1.0.2.0 US AiMesh，Country那里可以选择ALL，然后下载Target CFE即可得到自己的CFE，**重命名为new_cfe.bin待用**。

2\. 准备一个FAT32的U盘，将盘符改名为USB，下载[mtd-write.7z](/uploads/2019/03/mtd-write.7z)和[FW_RT_AC68U_30043763626.zip](/uploads/2019/03/fw_rt_ac68u_30043763626.zip)，将new_cfe.bin、mtd-write和FW_RT_AC68U_30043763626.trx三个文件打包为files12345.zip放入U盘中并插到路由器上。<!--more-->

### 开始刷写

登陆路由器(建议使用Chrome隐身模式，这样不会受到Chrome扩展的影响)，进入网络工具 --> 网络诊断，右键点检查，会出现开发者工具，点击console标签，开始漫长的输入命令过程。（输入每条命令后按回车，然后点击路由器网页中的“网络诊断”按钮，并建议把ping的地址由www.google.com改为国内网址）。

![](/uploads/2019/03/chrome-developer-console.png)

```javascript
validForm = function(){document.form.SystemCmd.value = "ping\necho hello world";return true;}    # 测试功能，如果正常路由器网页会显示“hello world”

validForm = function(){document.form.SystemCmd.value = "ping\nmount -t tmpfs tmpfs userRpm";return true;}
validForm = function(){document.form.SystemCmd.value = "ping\nmount";return true;}
validForm = function(){document.form.SystemCmd.value = "ping\ncp -a . userRpm";return true;}
validForm = function(){document.form.SystemCmd.value = "ping\nmount --move userRpm .";return true;}
validForm = function(){document.form.SystemCmd.value = "ping\nmount";return true;}
validForm = function(){document.form.SystemCmd.value = "ping\nservice restart_httpd";return true;}    # 等待httpd重启，似乎需要时间稍长一些

validForm = function(){document.form.SystemCmd.value = "ping\nwget -A txt -r -nH -nd --no-check-certificate tmac1900.heartnn.com";return true;}    # 这里我自己做了一个网站，原网站wget可能遭遇证书问题

validForm = function(){document.form.SystemCmd.value = "ping\n. u.txt "+encodeURIComponent('find /tmp/mnt -name files12345.zip').replace(/%/g,'..');return true;}    # 如果显示 sh: .: line 2: u.txt: not found，则证明上一步出现错误，正常的话files12345.zip应该可以挂载了

validForm = function(){document.form.SystemCmd.value = "ping\n. u.txt "+encodeURIComponent('unzip -o /tmp/mnt/USB/files12345.zip').replace(/%/g,'..');return true;}    # 解压缩

validForm = function(){document.form.SystemCmd.value = "ping\nchmod 755 mtd-write";return true;}    # 修改mtd-write权限

validForm = function(){document.form.SystemCmd.value = "ping\n. u.txt "+encodeURIComponent("./mtd-write new_cfe.bin boot").replace(/%/g,'..');return true;}    # 刷写CFE

validForm = function(){document.form.SystemCmd.value = "ping\nmtd-write2 FW_RT_AC68U_30043763626.trx linux";return true;}    # 刷写固件
```

上述步骤完成后等3分钟，关掉电源，按住WPS按钮不放同时开机，继续按住WPS按钮一直到路由后面白色灯闪烁，松开WPS按钮后路由自动重启，成功的话会清除nvram，等待路由重启，至此路由器就恢复好了，且支持AiMesh。

### 清除mtd5

通过系统管理打开ssh权限，用ssh工具登录路由器。

```bash
cat /dev/mtd5 > /jffs/mtd5_backup.bin
mkdir /tmp/asus_jffs
mount -t jffs2 /dev/mtdblock5 /tmp/asus_jffs
rm -rf /tmp/asus_jffs/*
sync && umount /tmp/asus_jffs
ln -s /sbin/rc mtd-erase
./mtd-erase -d asus    # 到这个步骤可能会卡住不动，可能是原帖作者恢复成功后又升级了固件
rm -rf /jffs/.sys/RT-AC68U
nvram unset fw_check && nvram commit && reboot
```

等待重启，到此大功告成，经测试可以正常升级到[3.0.0.4.384.45149](https://www.asus.com/us/Networking/RTAC68U/HelpDesk_BIOS/)(最好是断网升级)。

### 参考

1. <http://koolshare.cn/thread-137955-1-1.html>
2. <http://koolshare.cn/thread-136906-1-1.html>
3. <https://tmac1900.weebly.com/>

附自己搭建的u.txt：<https://heartnn.coding.net/p/tmac1900/d/tmac1900/git>
