---
title: "免ROOT免无障碍的安卓手机远控方案"
date: "2025-09-19T09:48:14+08:00"
categories: "Android"
tags: ["Android"]
author: "Baofeng Zhang"
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "基于甲壳虫助手+内网穿透实现基于ADB无线调试的安卓手机远程控制。"
canonicalURL: "https://canonical.url/to/page"
disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: false
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
cover:
    image: "<image path/url>"
    alt: "<alt text>" 
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
editPost:
    URL: "https://github.com/<path_to_repo>/content"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---

## 前期准备

先下载必要APP，方便后续步骤直接打开使用。

- 在控制端和被控端下载[贝锐蒲公英](https://pgy.oray.com/download/)或者[Tailscale](https://tailscale.com/download)等内网穿透工具。

- 在控制端下载[甲壳虫ADB助手](https://github.com/didjdk/adbhelper)。

另外，准备USB-to-TypeC的线数据线加OTG转接头，以及一个临时局域网。

## 局域网控制

在被控端打开开发者选项，找到USB调试，允许并打开。

在控制端打开甲壳虫ADB助手，使用USB-to-TypeC的线加OTG转接头连接两部手机，TypeC端连接被控端，OTG转接头连接控制端。

连接后被控机弹出调试提示，勾选永久允许。

控制机使用甲壳虫ADB助手建立连接，进入第二页实用工具，开启无线ADB调试。

拔掉数据线，确保被控机和控制机都在同一局域网下，尝试在甲壳虫ADB助手中输入IP地址建立连接，成功后可以在第三页远程控制中进行无线控制。

## 远程控制

被控机和控制机分别打开内网穿透工具并登录，找到其中显示的被控机IP，在控制端打开甲壳虫ADB助手输入该IP建立连接，即可实现远程控制。

## 其他

如果没有线材也可以直接尝试使用无线ADB调试连接，或许可以成功。

重启后开发者选项和调试功能可能自动关闭或重置，需要重新设置。

