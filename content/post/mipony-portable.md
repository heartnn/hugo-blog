---
title: 将Mipony打造成绿色版本
date: 2010-12-14T06:00:00+08:00
categories: 软件
tags: [bat]
---

首先声明的是，本文不只是提供了一个绿色软件，更是提供了一种方法而已。另外本文不是采取打包成单文件的形式，而是利用批处理文件来实现，从而让大家看得更明白。

[Mipony](http://mipony.net/)是一个网盘下载软件，可以自动分析多大数十种的网盘链接，从而达到自行下载的目的，省时省力。

![](/uploads/2010/12/mipony-support.png)<!--more-->

但是软件本身却不是绿色软件，设置要保存到用户目录里面，所以接下来就是绿化它了，网上有用虚拟打包的方法生成单文件的，但是不方便，新版本的时候就没办法了，除非自己会弄，要不然还要等他人更新，heartnn在这里说的是批处理的方法，简单好用，下面是几种方法，用哪种自己看着办 O(∩_∩)O哈哈~

再说方法之前，heartnn要说的是自己的实现方法，其实就是最笨的方法了，找到程序配置文件的位置，复制到程序目录里，然后做个批处理，让程序每次启动之前，把设置自己复制到用户目录里，程序结束后再复制回来～～(不知道我说明白了没。。。)

**以下方法如无说明，则是针对Windows 7实现的。**

再使用下面的任何方法时先对配置做个备份吧：

```dos
md .\Config
xcopy "%AppData%\MiPony\*" .\Config /E /y
```

## 方法一：最简陋的不是方法的方法

```dos
md "%AppData%\MiPony\"
xcopy .\Config\* "%AppData%\MiPony\" /E /y
MiPony.exe
xcopy "%AppData%\MiPony\*" .\Config /E /y
```

Windows XP或2003下的类似代码是：

```dos
md "%userprofile%\Application Data\MiPony\"
xcopy .\Config\* "%userprofile%\Application Data\MiPony\" /E /y
MiPony.exe
xcopy "%userprofile%\Application Data\*" .\Config /E /y
```

对看不懂代码的童鞋做一点小解释，首先是在用户目录中建立Mipony文件夹(其实真正的配置文件就在这里。)，然后把配置复制过去(就是前面备份在Config里的)，运行程序，程序结束后再复制回来。

这里指的注意的是，这款软件的配置是保存在当前用户目录里的，如果是所有目录的话，应该是`%allusersprofile%`(XP对应是`%ALLUSERSPROFILE%\Application Data`)。

问题来了，CMD窗口一直开着，不小心关掉怎么办，只能手工运行以下备份脚本了，但是如果忘了呢？下次启动程序的时候就不是最新的配置了。别急，继续方法二。

## 方法二：借助第三方程序

有个叫[HideApp](/uploads/2010/12/hideapp.7z)的小程序，可以使运行的程序隐藏，据说不是万能的，但对cmd窗口绝对有效。

假如我们刚才的批处理已经写成run.bat的话，现在把下面的命令存为批处理命令就可以了。

```dos
HideApp.exe run.bat
```

是不是很简洁了呢？唯一的缺点就是启动的时候会闪以下窗口。

另外，还有一个vbs脚本，也可以达到HideApp.exe的效果（不过这里heartnn没有测试呢）。

```vbscript
'HIDECMD.VBS    BY: fastslz    http://bbs.cn-dos.net    2007-01-03
'隐藏批处理运行窗口本身就是风险代码，如果你很菜请离开此页！

Usage1 = "请将批处理文件Bat、Cmd拖放到此文件上"
Usage2 = "命令语法１： HIDECMD.VBS  ""D:\Test.bat"""
Usage3 = "命令语法２： HIDECMD.VBS /M  ""D:\Test1.bat"" ""D:\Test2.bat"""
Usage4 = "隐藏批处理运行窗口工具"
Options = "/M"

set objShell = CreateObject ("Wscript.Shell")
set BatFile = WScript.Arguments
if BatFile.Count = 0 Then
    MsgBox Usage1 &vbCrLf& Usage2 &vbCrLf& Usage3, 4096+vbInformation, Usage4
    Wscript.Quit(0)
end if
if BatFile.Count > 1 Then
    ignoring = StrComp(BatFile(0), Options, vbTextCompare)
    if ignoring = 0 Then
        For I = 1 To BatFile.Count - 1
        MultiBat = BatFile(I)
        Call RunBat(MultiBat)
        Next
        Wscript.Quit(0)
    else
        objShell.Popup "参数太多或语法错误！"&vbCrLf&"此提示框在5秒后自动退出 ",5, "程序意外终止"
        Wscript.Quit(0)
    end if
end if
MultiBat=BatFile(0)
Call RunBat(MultiBat)
Wscript.Quit(0)

Function RunBat(MultiBat)
    BatTmp=split(MultiBat,"\")
    BatName=BatTmp(UBound(BatTmp))
    ignoring = StrComp(MultiBat, BatName, vbTextCompare)
    if ignoring = 0 Then
        BatPath=left(Wscript.ScriptFullName,len(Wscript.ScriptFullName)-len(Wscript.ScriptName))
    else
        BatPath=Left(MultiBat,Len(MultiBat)-Len(BatName))
    end if
    objShell.CurrentDirectory=BatPath
    objShell.Run ("%Comspec% /c "&Chr(34)&BatPath&BatName&Chr(34)),vbHide
End Function
```

如果你觉得足够了的话，就大可不必往下看了～～

## 方法三：从这里往下都是蛋疼的折腾。。。

使用vbs脚本，这个方法的通用性要比第二种方法小了，因为vbs脚本的安全性，很多杀毒软件都会误杀，不过如此简单的一个脚本，应该是没有什么大问题了。把下面的代码保存成一个vbs文件，试试效果吧（第一步的run.bat也在起作用，所以不要删掉。）。

```vbscript
Set ws = CreateObject("Wscript.Shell")
ws.run "cmd /c run_win7.bat",vbhide
```

这样就达到连闪以下窗口都没有了。

## 方法四：其实我比较简单～

使用在批处理中嵌入vbs的方法，使用中会产生一个vbs文件，程序结束后删除。把下面几行代码存为批处理文件： 

```dos
@if exist scucopy.vbs @goto findusb1
@echo Set ws = CreateObject("Wscript.Shell") >scucopy.vbs
@echo ws.run "cmd /c run.bat",vbhide >>scucopy.vbs
@start scucopy.vbs
@exit
:findusb1
md "%AppData%\MiPony\"
xcopy .\Config\* "%AppData%\MiPony\" /E /y
MiPony.exe
xcopy "%AppData%\MiPony\*" .\Config /E /y
del scucopy.vbs
```

这里要**注意**的是，第三行中的批处理名称要与这个批处理文件名相同。

这样一来，整个引导程序变成了一个文件，方便至极了。

## 方法五：再蛋疼的延伸一下～

很害怕启动时错误启动成Mipony.exe吗？把Mipony.exe和方法四中的run.bat制作成WinRAR自解压文件吧（用方法二里的方法也可以），当然要在run.bat结尾增加两条del命令，方便使用完毕后删除这两个文件。记得压缩后的exe文档不要叫Mipony.exe了，可以另外起名字 o(*≧▽≦)ツ 以下几个图代表这个方案的大概步骤：

![](/uploads/2010/12/pack1.png)

![](/uploads/2010/12/pack2.png)

![](/uploads/2010/12/pack3.png)

**无论是哪种方法，最重要的一点，如果关机的话，一定要先关掉程序才可以，否则的话，接下来的脚本将无法执行。**

最后总结一下：这其实算是笨办法了，估计也不是每个人都愿意这么折腾。

PS. 给大家个提示，用这种方法可以把CintaNotes弄成多数据库的～～

2010-12-27: 更新了方法四的错误之处。
