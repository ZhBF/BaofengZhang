---
title: "在Hugo的PaperMod中搜索"
date: "2025-08-02T11:27:16+08:00"
categories: "Hugo"
tags: ["Hugo"]
author: "Baofeng Zhang"
showToc: false
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "在Hugo的PaperMod主题中启用搜索功能。"
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

# 在Hugo的PaperMod中搜索

https://adityatelange.github.io/hugo-PaperMod/posts/papermod/papermod-features/#search-page

https://loyayz.com/website/220529-hugo-papermodx-add-search/

在`hugo.yml`中添加以下内容，确保其输出`public/index.json`，此文件为检索所需。

 ```yaml
 outputs:
   home:
     - HTML
     - RSS
     - JSON # necessary for search
 ```

在`hugo.yml`中，修改fusejs的一系列参数，可参照https://www.fusejs.io/api/options.html。

```yaml
params:
  fuseOpts:
    isCaseSensitive: false
    includeMatches: true
    findAllMatches: true
    shouldSort: true
    location: 0
    distance: 1000
    threshold: 0.4
    minMatchCharLength: 0
    limit: 10 # refer: https://www.fusejs.io/api/methods.html#search
    keys: ["title", "permalink", "summary", "content"]
```

在`hugo.yml`中，添加搜索选项到菜单栏。

```yaml
menu:
  main:
    - identifier: search
      name: search
      url: /search/
      weight: 5
```

创建搜索页面`content/search.md`，填入以下内容。

```yaml
---
title: "Search" # in any language you want
layout: "search" # necessary for search
# url: "/search"
# description: "Description for Search"
summary: "search"
placeholder: "placeholder text in search input box"
---
```

> 注意，在模板以及每一篇博客的`frontmatter`中，确保`searchHidden`选项为`false`，官方推荐配置中该选项默认为`true`，即不可被检索。

