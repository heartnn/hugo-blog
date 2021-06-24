---
title: "自动生成PSV RetroArch游戏列表"
date: 2019-06-25T19:02:18+08:00
categories: 游戏
tags: [retroarch,psv]
---

本版本为psvita破解吧[原有版本](https://tieba.baidu.com/p/5916455885)的增强版，可自动生成FC、SFC、MD、PCE、GBC、GBA、PS1列表，且按照名称排序，并在生成后借助iconv自动转换为UTF-8编码。

将下面代码和iconv.exe一起放在`ux0:/Roms`下即可，生成的列表在`ux0:/data/retroarch/playlists`下。

> iconv的Windows版本下载地址：<https://github.com/mlocati/gettext-iconv-windows/releases>，需下载static 64位版本并解压缩。

```bat
@echo off
setlocal EnableDelayedExpansion
set NAME1=GBA
set CORE1=vba_next_libretro.self
set NAME2=FC
set CORE2=fceumm_libretro.self
set NAME3=GBC
set CORE3=gambatte_libretro.self
set NAME4=MD
set CORE4=genesis_plus_gx_libretro.self
set NAME5=PCE
set CORE5=mednafen_pce_fast_libretro.self
set NAME6=SFC
set CORE6=snes9x2005_libretro.self
set NAME7=PS1
set CORE7=pcsx_rearmed_libretro.self

set dvr=%~d0
set pth1=%cd:~7%
set pth=%pth1:Roms%
set lstpth=data\retroarch\playlists

for /l %%i in (1,1,7) do (
set nam=!NAME%%i!
set cor=!CORE%%i!
dir /on /b !nam!>%temp%\tmp.txt
type nul>!dvr!\!lstpth!\!nam!.lpl
for /f "delims=" %%g in (%temp%\tmp.txt) do (
set n=%%g
set gn=!n:~,-4!
echo.ux0:/!pth!/!nam!/%%g>>!dvr!\!lstpth!\!nam!.lpl
echo.!gn!>>!dvr!\!lstpth!\!nam!.lpl
echo.app0:/!cor!>>!dvr!\!lstpth!\!nam!.lpl
echo.DETECT>>!dvr!\!lstpth!\!nam!.lpl
echo.DETECT>>!dvr!\!lstpth!\!nam!.lpl
echo.!nam!.lpl>>!dvr!\!lstpth!\!nam!.lpl
)
iconv -s -f GBK -t UTF-8 !dvr!\!lstpth!\!nam!.lpl > !dvr!\!lstpth!\temp.txt
del !dvr!\!lstpth!\!nam!.lpl
ren !dvr!\!lstpth!\temp.txt !nam!.lpl
echo 已生成!nam!列表
)
echo 已完成所有任务
pause
```