+++

date = '2025-07-30T18:09:06+08:00'
draft = true
title = 'Hugo 快速开始'

+++



# Hugo 快速开始

# 安装

使用命令安装hugo，此处以`chocolatey`​为例，运行命令`choco install hugo-extended`​。

安装完成后可以使用命令`hugo version`​查看hugo版本。

更新hugo可以使用命令`choco upgrade hugo-extended -y`​。

> 对于Windows用户：
>
> 不要使用 Command Prompt 或者 Windows PowerShell。使用 PowerShell 或者 Linux 终端，比如 WSL 和 Git Bash。（PowerShell 和Windows PowerShell 是不同的应用程序。）

# 建站

新建一个博客网站。

新建一个hugo文件夹并进入。

```bash
hugo new site MyHugoBlog
cd .\MyHugoBlog\
```

导入一个新的主题并设置应用该主题。

```bash
git init
git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke
echo "theme = 'ananke'" >> hugo.toml
hugo server
```

# 内容

使用新建一个页面。

```bash
hugo new content content/posts/my-first-post.md
```

使用编辑器打开文件编辑内容，文件会有如下的默认内容。

```bash
+++
title = 'My First Post'
date = 2024-01-14T07:07:07+01:00
draft = true
+++
```

Hugo默认不会对`draft = true`​的内容进行发布，在内容完成后设置为false。

在文件中添加内容用作测试，内容如下。

```bash
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

```bash
hugo server --buildDrafts
hugo server -D
```

# 配置

工程根目录的`hugo.toml`​为配置文件，其当前内容如下。

```bash
baseURL = 'https://example.org/'
languageCode = 'en-us'
title = 'My New Hugo Site'
theme = 'ananke'
```

根据情况修改配置。

# 发布

发布时，不包含任何的草稿、未来或过期内容（由文件头内容决定）。

使用以下命令发布。

```bash
hugo
```

# 托管

‍托管到 Github Pages。

本地使用git进行管理和提交。

在Github新建一个仓库，使用以下命令将本地内容推送到Github。

```
git remote add origin https://github.com/ZhBF/BaofengZhang.git
git branch -M main
git push -u origin main
```

找到Gihub仓库的`Settings > Pages > Build and deployment > Source`，设置为`Github Actions`。



在hugo.toml中添加以下内容，配置图片缓存目录。

```
[caches]
  [caches.images]
    dir = ':cacheDir/images'
```

















