---
title: "Git终端的中文乱码问题"
date: "2025-08-01T22:04:50+08:00"
categories: "Git"
tags: ["Git", "Git Bash"]
author: "Baofeng Zhang"
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "Desc Text."
canonicalURL: "https://canonical.url/to/page"
disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: true
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

# Git终端的中文乱码问题

对于git终端不显示中文而是显示八进制字符编码的问题

![image-20250730194646933](https://raw.githubusercontent.com/ZhBF/Images/main/images/image-20250730194646933.png)

可以使用如下命令解决。

```bash
git config --global core.quotepath false
```

