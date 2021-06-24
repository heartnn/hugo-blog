---
title: "紧急更新一个饭否用的Google Reader分享脚本"
date: 2010-11-26T06:32:00+08:00
categories: 网络
tags: [fanfou]
---

饭否复活了～～大快人心。

但是原来的饭否应用貌似大都不能用了，因为<http://api.fanfou.com>好像还没有恢复，包括以前的油猴脚本也不行了，但是我们还是需要分享一些东西的，所以这个脚本就诞生了。

如果你喜欢同步Reader Share的RSS的话，就不用往下看了，同样，这个脚本不是在手机上使用的。

打开Google Reader Setting，打开Send To标签，在最下面Create a custom link。

```plain
Name : 饭否
URL : http://fanfou.com/sharer?u=${short-url}&t=[GReader分享]${title}&d=&s=bm
Icon URL : http://static.fanfou.com/favicon.ico
```<!--more-->

其中`${title}`是标题，`${short-url}`是短地址，如果不喜欢短地址的话可以修改为`${url}`，另外`[GReader分享]`是我自己加的，可以自行修改或者去掉。

使用效果见下图：

![](/uploads/2010/11/fanfou-share.png)

得到的界面与饭否分享是一致的，因为api不能用，直接使用了饭否分享的界面。 

![](/uploads/2010/11/fanfou-share-settings.png)

如果选择`${short-url}`的话，Google会先给出一个Shorting URL的界面，然后跳转到上面的图，分享完毕后，页面会自动关闭。 祝大家在饭否玩的愉快。

- 我的饭否：<http://fanfou.com/heartnn>
- 我的饭否邀请：<http://fanfou.com/register/ZX5AQ6ECcRYf>
