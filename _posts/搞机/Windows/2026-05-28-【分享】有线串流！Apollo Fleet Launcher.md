---
title: 【分享】有线串流！Apollo Fleet Launcher
date: "2026-05-28T20:21:31+08:00"
lastmod: "2026-05-28T21:37:35+08:00"
description: 
categories: []
tags: []
---

- 前言：终于解决了困扰我许久的问题，现在可以USB有线串流了。太感谢这位了。

- 仓库地址：[https://github.com/drajabr/Apollo-Fleet-Launcher](https://github.com/drajabr/Apollo-Fleet-Launcher)
- 免责声明：项目并非本人开发，如遇到任何问题请去仓库，向作者提出issue。使用ADB造成的安全风险自负。
- 前置条件：电脑端使用Apollo(Sunshine未经过测试，且会出bug)，平板端使用Artemis/Moonlight均可。

## 项目简介

- 翻译：

  一个简单的工具，用于配置多个  **@ClassicOldSong/Apollo** 实例，实现**多显示器流式传输**模式，主要针对桌面使用场景——例如多台安卓平板可以作为**即插即用的外接显示器**使用。

  这与我的旧版 Multi-streaming-setup 脚本理念相同，但增加了 GUI（图形界面）和自动配置功能，并打包了安卓客户端所需的必要二进制文件。
- 原文

  A simple tool to configure multiple instances of [@ClassicOldSong/Apollo](https://github.com/ClassicOldSong/Apollo) for streaming multi monitor mode, mainly targeting desktop use case where multi devices like android tablets can be used as Plug and play external monitor.

  This is the same concept of my old [Multi-streaming-setup](https://github.com/drajabr/My-Sunshine-setup) scripts, with ease of GUI and Auto Configuration, bundled with necessary binaries for Android clients stuff.

## PC配置

### 十分建议使用Apollo

建议使用Apollo，否则你可能遇到和这位一样的[Issue #20 · drajabr/Apollo-Fleet-Launcher](https://github.com/drajabr/Apollo-Fleet-Launcher/issues/20)

![](/assets/img/media/2026_5_28/image-20260528212853-oici17v.png)

### 调整兼容性设置

前往[Release](https://github.com/drajabr/Apollo-Fleet-Launcher/releases)下载最新版本（本文使用v0.3.3)，安装好后需要按照图片修改兼容性设置，否则显示会有问题

![](/assets/img/media/2026_5_28/image-20260528202826-25gjwgp.png)

> 如果未修改兼容性设置，软件界面会显示异常（如下图）
>
> ![](/assets/img/media/2026_5_28/屏幕截图 2026-05-28 200653-20260528202943-6lpgvsz.png)

### 调整软件设置

先点击这个图标解锁

![](/assets/img/media/2026_5_28/image-20260528202534-q94pq01.png)

然后设置成你自己的Apollo安装路径，并且打开ADB反向网络共享。

![](/assets/img/media/2026_5_28/屏幕截图 2026-05-28 202557-20260528203116-1i9mjez.png)

另外，实例名称可以自定义。其他功能按需打开（我没试过）。

## Android设置

开发者选项里面打开USB调试，打开允许在仅充电模式下调试，然后允许你电脑进行调试即可。不会的网上一搜就有。

允许后会通过adb给你的Android设备安装GnirehtetX，对应仓库应该是[https://github.com/Linus789/gnirehtetx](https://github.com/Linus789/gnirehtetx)。GnirehtetX通过 ADB 建立一个 USB 虚拟网络隧道，让 Android 设备能通过 USB 线上网。*如果关闭 USB 调试，隧道断开，串流会退回 WiFi。*

然后平板会提示开启VPN连接，确认即可。

## 效果

现在就是有线串流了，你可以关闭wifi测试效果。网络/解码延迟波动下降至2~5ms。并且不会丢包。

WIFI串流，静态画面网络延迟还行，但一旦动起来（单纯鼠标指针拖一拖）会严重丢包（20%以上），延迟很容易上升到50ms以上，视频更是别想看。调整Apollo和Moonlight的效果有限。

![](/assets/img/media/2026_5_28/Screenshot_20260528_211348_com.limelight.noir-20260528211451-5f9jrgs.jpg)

通过Apollo Fleet项目+GnirehtetX有线串流，播放视频时平均丢包率降至10%以下，且基本不会出现明显卡顿。并且软件启动的Apollo实例用的还是全默认设置，没有调整过任何东西。

![](/assets/img/media/2026_5_28/Screenshot_20260528_205907_com.limelight.noir-20260528210051-n1m6eie.jpg)

## 后话

回头看看我要不要试试整个汉化的。

并且我还找到了[https://github.com/payne0420/Vibepollo-Fleet-Launcher](https://github.com/payne0420/Vibepollo-Fleet-Launcher)和[https://github.com/Nonary/Vibepollo](https://github.com/Nonary/Vibepollo)。Vibepollo是AI生成/管理的项目，但是解决了很多Apollo/Sunshine使用过程中遇到的痛点。Vibepollo-Fleet-Launcher则是Apollo-Fleet-Launcher的fork版本，对Vibepollo进行了适配。（本人没试过这两个的效果）

‍
