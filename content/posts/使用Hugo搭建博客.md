---
title: "使用Hugo搭建博客"
date: "2025-08-01T22:04:38+08:00"
tags: ["default"]
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

# 使用Hugo搭建博客

## 安装

使用命令安装hugo，此处以`chocolatey`​为例，运行命令`choco install hugo-extended`​。

安装完成后可以使用命令`hugo version`​查看hugo版本。

更新hugo可以使用命令`choco upgrade hugo-extended -y`​。

> 对于Windows用户：
>
> 不要使用 Command Prompt 或者 Windows PowerShell。使用 PowerShell 或者 Linux 终端，比如 WSL 和 Git Bash。（PowerShell 和Windows PowerShell 是不同的应用程序。）

## 建站

新建一个博客网站。

新建一个hugo文件夹并进入。

```powershell
hugo new site MyHugoBlog
cd .\MyHugoBlog\
```

导入一个新的主题并设置应用该主题。

```powershell
git init
git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke
echo "theme = 'ananke'" >> hugo.toml
hugo server
```

## 内容

使用新建一个页面。

```powershell
hugo new content content/posts/my-first-post.md
```

使用编辑器打开文件编辑内容，文件会有如下的默认内容。

```markdown
+++
title = 'My First Post'
date = 2024-01-14T07:07:07+01:00
draft = true
+++
```

Hugo默认不会对`draft = true`​的内容进行发布，在内容完成后设置为false。

在文件中添加内容用作测试，内容如下。

```markdown
+++
title = 'My First Post'
date = 2024-01-14T07:07:07+01:00
draft = true
+++

## Introduction

This is **bold** text, and this is *emphasized* text.

Visit the [Hugo](https://gohugo.io) website!
```

使用以下命令之一预览网站，包含草稿内容。

```powershell
hugo server --buildDrafts
hugo server -D
```

## 配置

工程根目录的`hugo.toml`​为配置文件，其当前内容如下。

```toml
baseURL = 'https://example.org/'
languageCode = 'en-us'
title = 'My New Hugo Site'
theme = 'ananke'
```

根据情况修改配置。

## 发布

发布时，不包含任何的草稿、未来或过期内容（由文件头内容决定）。

使用以下命令发布。

```powershell
hugo
```

## 托管

‍托管到 Github Pages。

本地使用git至少进行一次提交，确保main分支有效。

在Github新建一个仓库，使用以下命令将本地内容推送到Github。

```bash
git remote add origin https://github.com/ZhBF/BaofengZhang.git
git branch -M main
git push -u origin main
```

找到Gihub仓库的`Settings > Pages > Build and deployment > Source`，设置为`Github Actions`。

在hugo.toml中添加以下内容，配置图片缓存目录。

```toml
[caches]
  [caches.images]
    dir = ':cacheDir/images'
```

创建`.github/workflows/hugo.yaml`。

```powershell
mkdir -p .github/workflows
touch .github/workflows/hugo.yaml
```

文件内容如下。

```yaml
# Sample workflow for building and deploying a Hugo site to GitHub Pages
name: Deploy Hugo site to Pages

on:
  # Runs on pushes targeting the default branch
  push:
    branches:
      - main

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Allow only one concurrent deployment, skipping runs queued between the run in-progress and latest queued.
# However, do NOT cancel in-progress runs as we want to allow these production deployments to complete.
concurrency:
  group: "pages"
  cancel-in-progress: false

# Default to bash
defaults:
  run:
    # GitHub-hosted runners automatically enable `set -eo pipefail` for Bash shells.
    shell: bash

jobs:
  # Build job
  build:
    runs-on: ubuntu-latest
    env:
      DART_SASS_VERSION: 1.89.2
      HUGO_VERSION: 0.148.0
      HUGO_ENVIRONMENT: production
      TZ: America/Los_Angeles
    steps:
      - name: Install Hugo CLI
        run: |
          wget -O ${{ runner.temp }}/hugo.deb https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.deb
          sudo dpkg -i ${{ runner.temp }}/hugo.deb
      - name: Install Dart Sass
        run: |
          wget -O ${{ runner.temp }}/dart-sass.tar.gz https://github.com/sass/dart-sass/releases/download/${DART_SASS_VERSION}/dart-sass-${DART_SASS_VERSION}-linux-x64.tar.gz
          tar -xf ${{ runner.temp }}/dart-sass.tar.gz --directory ${{ runner.temp }}
          mv ${{ runner.temp }}/dart-sass/ /usr/local/bin
          echo "/usr/local/bin/dart-sass" >> $GITHUB_PATH
      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: recursive
          fetch-depth: 0
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v5
      - name: Install Node.js dependencies
        run: "[[ -f package-lock.json || -f npm-shrinkwrap.json ]] && npm ci || true"
      - name: Cache Restore
        id: cache-restore
        uses: actions/cache/restore@v4
        with:
          path: |
            ${{ runner.temp }}/hugo_cache
          key: hugo-${{ github.run_id }}
          restore-keys:
            hugo-
      - name: Configure Git
        run: git config core.quotepath false
      - name: Build with Hugo
        run: |
          hugo \
            --gc \
            --minify \
            --baseURL "${{ steps.pages.outputs.base_url }}/" \
            --cacheDir "${{ runner.temp }}/hugo_cache"
      - name: Cache Save
        id: cache-save
        uses: actions/cache/save@v4
        with:
          path: |
            ${{ runner.temp }}/hugo_cache
          key: ${{ steps.cache-restore.outputs.cache-primary-key }}
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

  # Deployment job
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

将本地的所有修改推送到Github仓库，找到Actions，等待状态从黄色转为绿色。

然后点开对应的commit，deploy下会有对应的访问链接。
