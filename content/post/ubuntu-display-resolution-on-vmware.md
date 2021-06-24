---
title: "一劳永逸调整Ubuntu在VMware下的显示分辨率"
date: 2019-11-21T19:37:09+08:00
categories: 技术
tags: [ubuntu]
---

最近在虚拟机上安装了Xubuntu，用来编译一些软件，但VMware提供的分辨率有限，连常用的1920x1080都没有，参考以前的方法来修改分辨率也有不一样的地方，记录下来，需要的朋友可以借鉴。

[Archlinux的维基](https://wiki.archlinux.org/index.php/Xrandr_%28简体中文%29#添加未被检测到的有效分辨率)上给出了比较明确的方法，比网上教大家加入到`~/.profiles`的方法要好的多。

在实际应用的时候，发现不存在`/etc/X11/xorg.conf`这个文件，继续Google，得到可以重新生成这个文件。

首先按Ctrl+Alt+F1，进入TTY，普通用户登录，这里需要注意的是在TTY的时候，小键盘的Num Lock其实是关闭的。<!--more-->

然后可以重新生成`xorg.conf`：

```bash
sudo service lightdm stop    # Ubuntu请使用gdm
sudo Xorg -configure    # 重新生成xorg.conf，位置在~/xorg.conf.new
sudo service lightdm start    # 重新启动lightdm
mv ~/xorg.conf.new /etc/X11/xorg.conf
```

然后利用`cvt`得到几个常用的分辨率：

```bash
$ cvt 1536 864
# 1536x864 59.94 Hz (CVT 1.33M9) hsync: 53.76 kHz; pclk: 109.25 MHz
Modeline "1536x864_60.00"  109.25  1536 1624 1784 2032  864 867 872 897 -hsync +vsync
$ cvt 1600 900
# 1600x900 59.95 Hz (CVT 1.44M9) hsync: 55.99 kHz; pclk: 118.25 MHz
Modeline "1600x900_60.00"  118.25  1600 1696 1856 2112  900 903 908 934 -hsync +vsync
$ cvt 1920 1080
# 1920x1080 59.96 Hz (CVT 2.07M9) hsync: 67.16 kHz; pclk: 173.00 MHz
Modeline "1920x1080_60.00"  173.00  1920 2048 2248 2576  1080 1083 1088 1120 -hsync +vsync
```

编辑`/etc/X11/xorg.conf`：

sudo mousepad /etc/X11/xorg.conf

将上面Modeline的内容插入到`Section "Monitor"`之中：

```plain
Section "Monitor"
	Identifier   "Monitor0"
	VendorName   "Monitor Vendor"
	ModelName    "Monitor Model"
	Modeline "1536x864_60.00"  109.25  1536 1624 1784 2032  864 867 872 897 -hsync +vsync
	Modeline "1600x900_60.00"  118.25  1600 1696 1856 2112  900 903 908 934 -hsync +vsync
	Modeline "1920x1080_60.00"  173.00  1920 2048 2248 2576  1080 1083 1088 1120 -hsync +vsync
EndSection
```

至于为什么有1536x864这个分辨率，是因为VMware Workstation在主机为1920x1080的Windows 10上不能完整显示900这个高度。

![](/uploads/2019/11/ubuntu-display-resolution.png)

---

参考：

- [Xrandr](https://wiki.archlinux.org/index.php/Xrandr_%28简体中文%29#添加未被检测到的有效分辨率)
- [虚拟机中的ubuntu怎么设置1920X1080分辨率](https://blog.csdn.net/u013122625/article/details/52967831)
- [Ubuntu通过重新生成/etc/X11/xorg.conf文件来调整分辨率](http://blog.chinaunix.net/uid-25909722-id-3019407.html)
