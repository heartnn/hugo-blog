---
title: 比较几款主流的笔记软件
date: 2018-08-23T10:17:31+08:00
categories: 软件
tags: [note]
---

最近整理笔记，把几个相对主流一些的笔记软件做了比较，也了解的他们的优缺点，在此详细说明一下，对选用可能起到一些帮助。

### 1. [Evernote](https://evernote.com/intl/zh-cn/) ([印象笔记](https://www.yinxiang.com/))

需要注意的是国内已经和国外分家了，国内的印象笔记已经开始支持Markdown(至本文发布前只有Mac端支持)。

优点：第三方支持很多，用户也很多。  
缺点：免费用户只能同步两个客户端，流量60MB；另外100000条笔记和250个笔记本的限制，即使是付费用户也受不了。

### 2. [有道云笔记](http://note.youdao.com/)

把这个放在第二介绍，是因为这次全部试用下来以后，发现还不错，自从支持Markdown以后，似乎加了不少分。

优点：对Markdown支持较为完善，Mathjax和Mermaid都没有问题。  
缺点：Markdown插入图片需要会员，而会员又是这几款软件中最贵的。<!--more-->

### 3. [为知笔记](http://www.wiz.cn/)

从开始付费后，被我抛弃了一段时间，最近充了一年会员，用后其实小问题也不少。

优点：会员价格相对便宜，客户端涵盖系统广。  
缺点：Windows客户端渲染较慢，Markdown不支持Mermaid，不支持插入附件(即使能拖放，显示在文章中的也只是个图片，无法点击)，另外Markdown编辑需借用插件，原生的没有预览支持。

### 4. [蚂蚁笔记](https://leanote.com/)

付费不考虑，下载了开源版本，Windows二进制文件+MongoDB，具体搭建方法有时间会详细写。完全可以做到便携化，可以放在Syncthing(或坚果云、微云、Onedrive等)里进行同步。

优点：暂时没有感觉有什么特别的优点。  
缺点：客户端半残废；网页端暂时不支持Mermaid(github上以后有人做出来了)；渲染结果甚至连行间距都很别扭，不知道css怎么调整的。

### 5. [VNote](https://tamlok.github.io/vnote/)

把这个也放进来，是因为之前一直用它，放在Syncthing里，也还算是方便，不过手机端就傻眼了。顺便发一个便携化的bat，和VNote.exe放到一起执行(笔记在当前目录下的vnotebook)：

```dos
for /f "tokens=2,*" %%i in ('reg query "HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\Shell Folders" /v "Appdata"') do (
set user_appdata=%%j
)
rd /s /q "%user_appdata%\vnote"
rd /s /q "C:\vnotebook"
mklink "%user_appdata%\vnote" /J %~dp0config
mklink "C:\vnotebook" /J %~dp0vnotebook
```

优点：对各种语法支持非常全面，插入图片也做的很方便。  
缺点：Qt架构运行效率不高；手机端虽然可以直接查看md文件，但是还是不方便。

### 总结

关于流畅度和内存占用，以Windows客户端为基准(我的Mix2s跑哪个Android客户端都很流畅，完全测试不出来……)，排除掉蚂蚁笔记，因为那个完全看服务器的性能：

```
客户端流畅度: 有道云笔记 >  印象笔记 >  为知笔记 >= VNote
Markdown渲染: 有道云笔记 >  为知笔记 >> VNote       # 印象笔记暂时还不支持
内存占用:     为知笔记   << 印象笔记 <= VNote    <  有道云笔记
```

- 有道云是最流畅的，在CPU占用100%的情况下，切换笔记及渲染也都非常快，但是后台服务相对较多。
- 期待印象笔记的改版。
- 为知各方面还算是厚道的，不过一刀砍以后用户少了很多。
- 蚂蚁笔记并没有宣传的那么美好，其实挺简陋的。
- VNote的强大需要付出很多代价。比如抛弃在手机上看笔记，略慢的渲染速度等等。

综上，如果不考虑会员价格，那么选有道云；注重隐私选Evernote；其它均衡考虑选为知笔记。
