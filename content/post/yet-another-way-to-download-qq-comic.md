---
title: 另类下载腾讯漫画的方法
date: 2019-03-26T21:02:18+08:00
categories: 网络
tags: [idm,chrome]
---

首先说明此方法不能下载付费章节，另外此方法不含任何破解成份。

需要的工具：

1. Chrome浏览器(大家应该都有)；
2. 一款方便顺手的文本编辑器，我用EditPlus；
3. IDM下载器，或者类似工具；
4. 一款文件重命名软件，比如[菲菲更名宝贝](http://www.ffhome.com/works/1406.html)；
5. 类似XnView的可以对图片批量重命名的软件(非必须，但是更方便)。

正题开始，举例说明，这里以[《狐妖小红娘》](https://ac.qq.com/Comic/comicInfo/id/518333)为例。在网址前加入`m.`，这样你会得到对应的手机版网站，点击你所需要的章节，得到一个类似<https://m.ac.qq.com/chapter/index/id/518333/cid/10>的网址，518333代表这个漫画的编号，10是目前章节的编号。

右键点图片，点下面的检查，会打开开发者控制台，灰色代表目前图片所在的源代码位置，往上一些会有一个“漫画图片列表插入位置”，右键点下面的`<section class="comic-pic-list-all">`，选择"copy"-->"copy element"，然后新建一个文本文件，粘贴，这样你就得到了这一章节的漫画图片源代码。<!--more-->

![](/uploads/2019/03/qq-comic-chrome-elements.gif)

接下来需要查找替换掉每一个链接最后的800，这样你才能得到原始图片，否则有些图片横排的时候，只能得到小图，即便都是800px的图片，大小也不相同。

![](/uploads/2019/03/qq-comic-source-code.gif)

上面这张图中，data-cid="10"`代表章节编号，`data-index="1"`代表这是第一张图，`data-pid=代表这张图的编号。

接下来打开IDM，选择“任务”-->“导入”-->“从文本文件导入”，然后按照图中选择，全选后取消那个apk链接和itunes的网页，并且务必勾上下面的隐藏重复文件，确定后开始下载，下载完成的文件一般在用户目录的下载文件夹。

![](/uploads/2019/03/qq-comic-idm.gif)

打开菲菲更名宝贝，把下载完的html文件拖入。首先更改扩展名，点击“E.扩展名变更”标签：(下面每一步勾选完后需要点击工具栏里绿色的对勾才算更改成功。)

1\. 先清空现在的扩展名：

![](/uploads/2019/03/ffrename01.png)

2\. 然后把所有的jpg_2改为jpg，因为上面导入的网址每一个图片都是两个地址，这就是上面为什么要隐藏重复文件。

![](/uploads/2019/03/ffrename02.png)

3\. 然后切换到“N.文件名变更”，将文件名前面的42个字符删掉。

![](/uploads/2019/03/ffrename03.png)

4\. 到此，应该可以得到一些很简单的文件名了，至此这一掌节就算下载成功了文件名对应的正好是`data-pid`，强迫症可以用XnView之类的对图片重新编号。

![](/uploads/2019/03/qq-comic-file-list.png)

利用手机网页版可以连续查看章节，可以在打开一个章节后一直向下拖动，这样可以得到多个章节的下载列表，下载完毕后再统一分类，需要多注意`data-cid`、`data-index`和`data-pid`这三个参数即可。

关于下载后的jpg可能在Windows下不现实缩略图，这里我采用的是一款叫[ExifTool](https://sourceforge.net/projects/exiftool/files/)的软件，下载后，将exe文件重命名为`exiftool(-k -r -charset filename= -all= -overwrite_original).exe`，然后把图片拖放进去就可以了，清除了exif信息后，Windows会重新生成缩略图，用这个方法也可以清除图片中的无用信息。

![](/uploads/2019/03/qq-comic-file-thumb.gif)
