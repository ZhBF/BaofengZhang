---
title: "在WSL上部署自托管Overleaf"
date: "2025-09-19T17:26:05+08:00"
categories: "Application"
tags: ["Application"]
author: "Baofeng Zhang"
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "在WSL上部署自托管Overleaf。"
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

尝试在windows上部署自托管overleaf，但是由于windows的docker是基于wsl2的，因此转为直接在wsl2上进行部署。以下部署过程全程使用wsl2的debian系统。中间可能遇到网络问题，自行准备魔法工具或者搜索可用镜像源。

## Step 0: Update && Upgrade

运行以下命令以确保最新。

```bash
sudo apt update && sudo apt full-upgrade
```

## Step 1: Install Docker

依次运行以下指令，安装docker。

```bash
sudo apt install apt-transport-https ca-certificates curl gnupg2 software-properties-common
sudo curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=$(dpkg --print-architecture)] https://download.docker.com/linux/debian $(lsb_release -cs) stable"
sudo apt update
sudo apt install docker-ce docker-compose
```

使用以下命令确认docker已经成功运行。如果能看到active字样，说明成功。

```bash
systemctl status docker
```

使用以下命令确认docker-compose安装成功。如果能看到版本号，说明成功。

```bash
docker-compose --version
```

## Step 2: Docker Mirror

在使用docker安装overleaf的过程中，极易产生网络问题，即使是在魔法环境下都有时会比较艰难。因此建议配置Docker镜像源。本步骤也可以先跳过，等确定遇到问题后再进行配置也可。

使用编辑器打开（如果没有就新建）`/etc/docker/daemon.json`，添加以下内容。其中的地址可能已经失效，自行查找最新的docker镜像地址进行替换。

```bash
{
  "registry-mirrors": [
          "https://docker.sunzishaokao.com",
          "https://docker.xuanyuan.me/",
          "https://docker.1ms.run",
          "https://docker.1panel.live",
          "https://hub.rat.dev",
          "https://docker.wanpeng.top",
          "https://doublezonline.cloud",
          "https://docker.mrxn.net",
          "https://docker.anyhub.us.kg",
          "https://dislabaiot.xyz",
          "https://docker.fxxk.dedyn.io",
          "https://docker-mirror.aigc2d.com"
  ]
}
```

运行以下命令重启docker服务。

```bash
sudo systemctl daemon-reload
sudo systemctl restart docker
```

运行以下命令查看镜像是否配置成功。如果成功，应该可以在输出中找到前面配置的镜像地址。

```bash
docker info
```

另外，也可以尝试使用docker拉取一些包进行测试。例如`docker pull ubuntu`。

## Step 3: Install Overleaf

从github克隆仓库到本地目录并进入，后续所有命令都在该目录下执行。

```bash
git clone https://github.com/overleaf/toolkit.git ./overleaf
cd overleaf
```

运行以下指令创建本地配置文件。配置成功后，能够在`config`文件夹下看到配置文件。

```bash
sudo bin/init
```

在自动生成的配置文件中，默认的IP地址是127.0.0.1，默认的端口是80。根据个人需要，可以使用编辑器（例如vim）打开并进行修改。以下是修改示例。

```bash
OVERLEAF_LISTEN_IP=0.0.0.0  # 这将允许直接访问服务器本地IP上的Overleaf。
OVERLEAF_PORT=80  # 如果80冲突，可以修改为其他端口，比如9000。
```

运行以下指令开始安装overleaf。如果连接不到docker服务器的话，这一步会失败。开启魔法网络环境或者寻找国内最新可用的docker镜像源地址。

```bash
sudo bin/up
```

等到所需内容安装完毕并且终端中开始快速输出一些重复内容时，可以按下`Ctrl-C`退出。

然后，现在应该已经可以使用以下命令启动Overleaf。

```bash
bin/start
```

首次启动时，需要创建管理员账户。

打开浏览器并进入网址`http://your-ip-here/launchpad`，然后进行注册登录即可。如果是在本机wsl2上部署的，打开的网址是`http://127.0.0.1/launchpad`。

在此后需要登录Overleaf时，可以直接进入网址`http://your-ip-here/login`。如果是在本机wsl2上部署的，打开的网址是`http://127.0.0.1/login`。

另外，在使用完毕后，有需要时，可以随时使用以下命令停止Overleaf，但这并不是必须的。

```bash
bin/stop
```

完成以上操作后，在overleaf中新建测试文件已经能够成功编译。但是，为了节省带宽，Overleaf 镜像仅附带 TeX Live 的最小安装，含中文的项目文件此时仍然不能成功编译。

## Step 4: Update TeX Live

运行以下命令，进入Overleaf-docker-container。

```bash
bin/shell
```

然后，在其中依次执行以下命令，安装完整版texlive。

```bash
wget https://mirror.clientvps.com/CTAN/systems/texlive/tlnet/install-tl-unx.tar.gz
tar -xf install-tl-unx.tar.gz
cd install-tl-2*
perl install-tl
```

安装过程中会有选项选择，输入`I`即可。之后会看到需要安装四千多个包，耗费至少半个小时，耐心等待即可。

安装完成后，运行以下命令，更新 texlive 相关的链接路径。注意，此教程写于2025年春，此时 overleaf 中自带 texlive 2024，安装的最新版 texlive 为 2025，二者位于不同文件夹下，注意使用正确最新路径下的tlmgr运行命令。

```bash
# 注意，一定要确认用的是最新版的texlive的安装路径下的tlmgr命令
/usr/local/texlive/2025/bin/x86_64-linux/tlmgr path add
```

上述操作完成后，输入`exit`退出Overleaf-docker-container，可以重启一下服务以确保生效。

至此，自托管Overleaf完成部署。


## Step 5: Automatic Startup

在个人使用时，发现Overleaf服务似乎会在linux系统启动后默认自动启动，不需要额外配置。没有在文档中看到相关说明，但是如果发现没有自动启动，可自行利用`systemd`进行配置。

然后，使用以下方法使wsl2开机自启动且后台常驻。

使用以下命令查看并找到想要开机自己的发行版本。后续示例中假设为`Debian`。

```bash
wsl -l -v
```

`win+R`打开运行窗口，输入`shell:startup`打开启动文件夹。

新建文本文档，命名为`startup-debian.vbs`。文件名可自定义，扩展名一致即可。

在文件中输入以下内容并保存，可根据个人需要修改命令参数。其作用是启动wsl并且不显示窗口。

```bash
cmd="wsl -d Debian"
CreateObject("Wscript.Shell").run cmd,vbhide
```

大功告成，可以愉快写论文啦。
