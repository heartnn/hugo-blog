---
title: 解决群晖File Station中文拼音排序问题
date: 2021-11-17T12:18:42+08:00
categories: 技术
tags: [synology]
---

方法来自：<https://blog.icedream.xyz/2020/01/01/破解群晖file-station中文未按拼音排序问题/>

以下基于DSM 6.23-25426 Update 3 (x64)

涉及修改文件为`/usr/lib/libsynocore.so.6`。

`ucol_open`涉及到以下代码位置：

```
LOAD:000000000001650F                 lea     rsi, [rsp+28h+var_1C]
LOAD:0000000000016514                 lea     rdi, aPStartAddress+12h ; ""
LOAD:000000000001651B                 call    _ucol_open
```

具体修改操作：

1. 将`48 8D 3D 78 A9 00 00`替换成`48 8D 3D 7B AF 00 00`。
2. 查找字符串`string_join.c`，并替换成`string_joinzh`。

结论：这样修改完成后，在File Station中按拼音排序时，中文目录会排列到所有英文目录前。<!--more-->

提供一个[改好的文件](/uploads/2021/11/libsynocore.so.6.7z)，用SSH登录管理员权限替换即可，替换时请注意版本及文件权限。
