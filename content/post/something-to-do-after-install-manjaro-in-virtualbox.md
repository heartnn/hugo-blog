---
title: "Virtualbox中安装Manjaro Linux后需要做的一些事"
date: 2018-11-21T07:43:27+08:00
categories: 技术
tags: [linux,virtualbox]
---

最近爱上了[Manjaro](https://manjaro.org/)这个Linux发行版，尤其是更新到18.0以后，界面看着更舒服了，基于ArchLinux，，自带AUR，用起来非常方便。默认提供了Xfce、KDE、GNOME几种桌面，不过社区版集成了几乎所有主流的桌面。

## 一 Guest Additions问题

我安装的Xfce 64bit版本，安装完毕启动Virtualbox后，首先就是共享剪贴板和共享文件夹不能用，查[wiki](https://wiki.manjaro.org/index.php?title=Virtualbox#Guest_Additions)上明白的写着已经集成Guest Additions，但是却完全不能用。经过Google搜索后在Manjaro论坛上有了答案：

### 1 安装缺少的组件

在“添加/删除软件”中搜索`virtualbox-guest-utils`，或命令行运行：

    sudo pacman -S virtualbox-guest-utils

这里搜索`virtualbox`可以看到当前内核对应的guest-modules已经安装了，但是工具包默认没有装。内核模块应该是提供了驱动之类的，共享文件夹什么的只能靠工具包了。<!--more-->

### 2 启动服务以及加入用户控制

```bash
sudo systemctl enable vboxservice
sudo mkdir /media
sudo gpasswd -a $USER vboxsf
```

重启后应该就可以用了。

## 二 proxy问题

### 1 客户端

SS请直接安装`shadowsocks-qt5`，下面仅说明SSR的相关问题，先打开AUR源。

打开“软件包管理器”的首选项，打开开关，构建目录选`/tmp`就可以，或者它自己会生成。

![](/uploads/2018/11/manjaro-aur-support.png)

然后搜索`shadowsocksr`，会在AUR中出现`electron-ssr`，这个目前是图形界面的唯一选择，自带http代理和pac。经过较长时间的编译安装以后就可以用了，区别和Windows上的SSR不大，而且如果桌面是GNOME的话，启动完毕会自动把代理设置好(我的GNOME不显示ssr的小飞机，不知道为什么)。

### 2 PAC自动配置

点右下角的网络管理-->编辑连接-->Proxy，方法选`Auto`，PAC URL填`http://127.0.0.1:2333/proxy.pac`。

![](/uploads/2018/11/manjaro-proxy.png)

Firefox在设定使用系统代理就可以用了(或者直接将pac地址填到Firefox里)。千万不要手动设定`http_proxy=http://127.0.0.1:12333`，流量烧不起。。。

关于Chrome代理，比较头疼的就是这个了，无论是Chromium，还是AUR里的google-chrome，都不能使用上面的方法设定pac代理，[gconftool-2](https://www.webscalability.com/blog/2012/09/use-proxy-pac-with-chrome-xfce/)也不起作用，网上唯一靠谱的就是在Chrome启动加参数`--proxy-pac-url="http://127.0.0.1:2333/proxy.pac"`。

不要尝试安装`gnome-control-center`，运行不起来的，而且会带来更多的软件包。

如果是GNOME版本的话不用折腾就能用了，Xfce下有什么更好的办法请告诉我。

验证是否代理成功，可以打开<chrome://net-internals/#proxy>，如果写着`PAC script: http://127.0.0.1:2333/proxy.pac`就是成功了。

参考资料：

- [Manjaro as guest in VirtualBox, Shared Folder?](https://forum.manjaro.org/t/manjaro-as-guest-in-virtualbox-shared-folder/41675)
- [Proxy settings on GNOME3](https://wiki.archlinux.org/index.php/Proxy_server#Proxy_settings_on_GNOME3)
- [Tech Note: Command Line for Alternate PAC file for Chrome](https://etherealmind.com/tech-note-command-line-for-alternate-pac-file-for-chrome/)
