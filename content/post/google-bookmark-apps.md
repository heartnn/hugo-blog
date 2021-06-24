---
title: "细说Google Bookmark应用"
date: 2011-01-06T21:18:00+08:00
categories: 网络
tags: [bookmark,google,delicious]
---

![](/uploads/2011/01/google-bookmarks.png)

首先是Google书签的Bookmarklet：

```js
javascript:(function(){var a=window,b=document,c=encodeURIComponent,d=a.open('https://www.google.com/bookmarks/mark?op=edit&amp;output=popup&amp;bkmk='+c(b.location)+'&amp;title='+c(b.title),'bkmk_popup','left='+((a.screenX||a.screenLeft)+10)+',top='+((a.screenY||a.screenTop)+10)+',height=420px,width=550px,resizable=1,alwaysRaised=1');a.setTimeout(function(){d.focus()},300)})();
```<!--more-->

有点麻烦的是，如果想批量导入书签的话，必须使用Google Toolbar来完成，这是比较别扭的，不过好在有了下面的应用，可以借助Delicious来完成，这样的话，Google书签和Delicious就可以双向导入导出了。

[开源项目](https://github.com/mihaip/delicious2google/)：[从Delicious导入Google Bookmark](https://delicious-export.appspot.com/)

以下是Google书签在浏览器中的应用：（并非在浏览器中只能用这个，而是比较好用而已。）

**Firefox:**

[GMarks](https://addons.mozilla.org/zh-TW/firefox/addon/2888/)

**Chrome:**

[Yet Another Google Bookmarks Extension](https://chrome.google.com/extensions/detail/jdnejaepfmacfdmhkplckpfdcjgbeode)

**Opera:**

[Add to Google bookmarks](https://addons.opera.com/addons/extensions/details/add-to-google-bookmarks/1.0.3/?display=en)

另外对于Opera浏览器的按钮，可以写成这样：

```js
opera:/button/Go to Page,"javascript:(function(){var b=document,c = encodeURIComponent,d=open('http://www.google.com/bookmarks/mark?op=edit&amp;output=popup&amp;bkmk='+c(b.location)+'&amp;title='+c(b.title),'bkmk_popup','height=420px,width=550px')})()"
```
