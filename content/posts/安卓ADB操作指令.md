---
title: "安卓ADB操作指令"
date: "2025-08-23T23:17:17+08:00"
categories: "Android"
tags: ["Android"]
author: "Baofeng Zhang"
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "常用的安卓ADB操作指令。"
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



模拟HOME键

```
adb shell input keyevent 3
```

模拟BACK键

```
adb shell input keyevent 4
```

模拟APP_SWITCH键

```
adb shell input keyevent 187
```



模拟电源键

```
adb shell input keyevent 26
```



提高音量

```
adb shell input keyevent 24
```

降低音量

```
adb shell input keyevent 25
```



模拟点击 `(xxx, yyy)`

```
adb shell input tap xxx yyy
```



模拟滑动 `(xxx1, yyy1) -> (xxx2, yyy2)`

```
adb shell input swipe xxx1 yyy1 xxx2 yyy2
```



模拟输入文本 `123456`

```
adb shell input text 123456
```



