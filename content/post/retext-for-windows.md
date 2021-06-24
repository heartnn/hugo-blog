---
title: 在Windows下使用ReText
date: 2013-05-10T23:17:00+08:00
categories: 软件
tags: [windows,python]
---

网上关于[ReText](http://sourceforge.net/p/retext/)的介绍都是MarkDown编辑器，其实是[reStructuredText](http://docutils.sourceforge.net/rst.html)的跨平台编辑器（使用[Python](http://www.python.org/)开发），只是大家可能用MarkDown多一些。

其实[ReText官方Wiki](http://sourceforge.net/p/retext/wiki/Windows%20Install%20of%20ReText/)也说怎么在Windows下安装了，只是有些许错误，又碍于英文，在这里heartnn说一下自己的安装过程，可能比官方的简洁一些。

首先你的电脑里要安装[Python 3.3](http://www.python.org/getit/)以上的版本，[ReText](http://sourceforge.net/p/retext/)是这样推荐的，不知道低版本会怎样，如果电脑里有Python 2.7的话，建议先卸载掉再安装新版。

然后安装[PyQt 4](http://www.riverbankcomputing.co.uk/software/pyqt/download)对应Python 3.3的版本。<!--more-->

上面两个软件我都是用的默认路径，如果安装时更改了路径的话，下面请自助修正路径。

接下来是修改环境变量：右键单击我的电脑--\>属性--\>高级--\>环境变量 新建系统变量：

```plain
PYTHONPATH=C:\Python33\Lib\;C:\Python33\Lib\site-packages\
```

![](/uploads/2013/05/python-path.png)

然后修改PATH变量，在最前面加入：

```plain
C:\Python33;C:\Python33\Lib\site-packages\PyQt4\bin;
```

然后下载运行[distribute_setup.py](http://python-distribute.org/distribute_setup.py)，会安装distribute package，这样在下面可以应用easy-install命令。

然后打开cmd窗口，`cd \Python33\Scripts`，然后安装相关组件：

```dos
easy_install Pygments
easy_install ElementTree
easy_install Markdown
easy_install Markups
```

至此，ReText的预备工作完成。

最后下载[ReText](http://sourceforge.net/p/retext/files/latest/)和相关的[图标包](http://sourceforge.net/projects/retext/files/Icons/)，解压缩后把图标包放在ReText目录的icons目录下，然后双击retext.py，大功告成。

![](/uploads/2013/05/retext.png)

如果需要便携版的话，官方也提供了一个批处理文件，比如可以把刚才部署好的环境`C:\Python33`放到`U:\Python33`，连同ReText一起copy到U盘里，然后新建一个下面的批处理文件即可。

```dos
REM ReText Startup batch file
REM -------------------------
REM determine drive letter of batchfile
for /f "delims=\" %%d in ('cd') do set curdrv=%%d
echo %curdrv%
REM Set ENVs using current drive letter if ENVs not set for USB Sticks
REM REM them out if you have the ENVs set
set PYTHONPATH=%curdrv%\Python33\Lib;%curdrv%\Python33\Lib\site-packages
set PATH=%curdrv%\Python33;%curdrv%\Python33\Lib\site-packages\PyQt4\bin;%PATH%
REM Start ReText
start /B %curdrv%\Python33\pythonw.exe %CD%\retext.py
```
