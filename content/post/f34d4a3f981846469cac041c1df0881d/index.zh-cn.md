+++
author = "FlintyLemming"
title = "群晖 Docker 安装 qBittorrent (DSM 7.2)"
slug = "f34d4a3f981846469cac041c1df0881d"
date = "2023-08-17"
description = ""
categories = ["HomeLab", "MineService"]
tags = ["qBittorrent", "群晖"]
image = "https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/08/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qBittorrent%20%28DSM%207.2%29/philip-oroni-mb_VqCIOTiU-unsplash.avif"
+++

## 准备工作

### 下载镜像

1. 打开 Container Manager，左边点开 注册表，搜索 qbittorrent。找到 linuxserver/qbittorrent 后，双击。标签保持 latest，点击 应用 开始下载镜像
    
    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/08/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qBittorrent%20%28DSM%207.2%29/Untitled.avif)
    
2. 转到 映像，等待下载完毕
    
    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/08/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qBittorrent%20%28DSM%207.2%29/Untitled%201.avif)
    

### 创建文件夹

需要创建至少两个文件夹，一个文件夹用来保存 qbittorrent 运行时使用的配置文件，一个用来保存下载的内容

我这边按照自己的习惯分别在两个不同的共享文件夹中创建了两个文件夹

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/08/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qBittorrent%20%28DSM%207.2%29/Untitled%202.avif)

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/08/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qBittorrent%20%28DSM%207.2%29/Untitled%203.avif)

### 查找当前用户的 uid 与管理员用户组的 gid

为了保证 qbittorrent 的容器对于我们创建的文件夹有正常的读写权限，避免需要对文件夹无脑 chmod 777，后面在启动容器时需要指定所使用的 uid (User ID) 和 gid (Group ID)。

一般来说群晖第一个账号的 **uid 为 1026**，管理员用户组的 **gid 为 101**。你也可以按照下面的步骤检查：

1. 在 控制面板 - 终端机和 SNMP - 终端机 中勾选 启动 SSH 功能，并修改端口为非 22 的其他端口，最后点击应用
    
    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/08/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qBittorrent%20%28DSM%207.2%29/Untitled%204.avif)
    
2. 打开电脑上的 Terminal。Windows 10 可以右键开始菜单，然后点击 Windows PowerShell；Windows 11 可以右键开始菜单，然后点击 终端；macOS 可以打开 终端 App。
    
    输入下面的命令通过 SSH 连接到群晖
    
    ```bash
    ssh <你的用户名>@<你的群晖IP地址> -p <你设置的端口>
    ```
    
    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/08/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qBittorrent%20%28DSM%207.2%29/Untitled%205.avif)
    
3. 第一次链接会让你确认连接的设备，输入 yes。然后输入你的密码，输入时不会显示任何内容，输入完直接回车就行。
    
    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/08/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qBittorrent%20%28DSM%207.2%29/Untitled%206.avif)
    
    能看到 NAS 的账号名称和计算机名称后，说明就连到了 DSM 的 Shell 了
    
4. 此时输入 `id` 并回车，就可以看到当前用户的 uid （1026）和管理员组的 gid 了（101）
    
    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/08/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qBittorrent%20%28DSM%207.2%29/Untitled%207.avif)
    

## 启动容器

1. 点击 映像 中刚下好的镜像，然后点击运行
    
    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/08/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qBittorrent%20%28DSM%207.2%29/Untitled%208.avif)
    
2. 容器名称可以改一改，我自己的习惯是在前面加上这个容器的 Web UI 所用的端口。如果想要启动 DSM 时能够自动启动 qBittorrent，就勾上 启动自动重新启动。然后点击 下一步。
    
    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/08/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qBittorrent%20%28DSM%207.2%29/Untitled%209.avif)
    
3. 群晖的 Docker bridge 网络默认是没有 ipv6 的。修改起来也比较麻烦，需要改 Docker 的配置文件，要添加 `fixed-cidr-v6` 参数以设置 ipv6 子网。由于本次教程选用的 qBittorrent 镜像本身可以通过修改环境变量以设置启动时使用的端口，所以可以直接使用 host 网络。
    
    选用 host 网络就不需要额外配置映射，这两条可以删除。
    
    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/08/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qBittorrent%20%28DSM%207.2%29/Untitled%2010.avif)
    
4. 设置映射刚才创建的两个文件夹。保存配置文件的文件夹映射容器的路径为 `/config/qBittorrent` ；保存下载内容的文件夹映射容器的路径为 `/downloads`
    
    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/08/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qBittorrent%20%28DSM%207.2%29/Untitled%2011.avif)
    
    配置文件夹不建议按照官方说明直接映射 /config ，否则会出现套娃的现象。而若映射上一层的文件夹又并不够安全。
    
    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/08/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qBittorrent%20%28DSM%207.2%29/Untitled%2012.avif)
    
5. 环境 这里创建四个变量。PUID 和 PGID 分别输入刚才找到的 uid 和 gid；TZ 输入 `Asia/Shanghai` ；WEBUI_PORT 输入你要设置的 Web UI 端口
    
    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/08/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qBittorrent%20%28DSM%207.2%29/Untitled%2013.avif)
    
6. 网络 修改为 host，然后 下一步
    
    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/08/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qBittorrent%20%28DSM%207.2%29/Untitled%2014.avif)
    
7. 确认下没什么问题就点 完成
    
    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/08/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qBittorrent%20%28DSM%207.2%29/Untitled%2015.avif)
    
8. qBittorrent 新版本默认密码不再是 adminadmin，而是默认生成的密码，所以需要打开日志找到密码。先点一下容器名字进入容器详情

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/08/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qBittorrent%20%28DSM%207.2%29/CleanShot%202024-03-27%20at%2018.12.54%402x.avif)

9. 然后找到这行日志，他会告诉你默认密码是多少，用这个密码登录

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/08/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qBittorrent%20%28DSM%207.2%29/CleanShot%202024-03-27%20at%2018.10.40%402x.avif)

## 设置 qBittorrent

### 语言和密码

根据你自己设置的端口，打开 Web UI。默认账号密码分别为 admin 和 adminadmin。登录后打开设置，先把界面语言和密码改了。

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/08/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qBittorrent%20%28DSM%207.2%29/Untitled%2017.avif)

### 下载与上传设置

监听端口就是上传的端口，需要在路由器中配置端口映射。不建议使用 UPnP 映射，大部分路由器后台对于 UPnP 的映射管理都非常不好。

连接限制我因为是 SSD 刷 pt，所以我全部都关了。如果使用 HDD，完全取消限制并不能使速度最大化，反而会增加随机读写的I/O队列，让上传和下载速度更慢，需要自己根据情况设置。

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/08/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qBittorrent%20%28DSM%207.2%29/Untitled%2018.avif)

Torrent 排队 同理，需要根据自己的条件设置

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/08/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qBittorrent%20%28DSM%207.2%29/Untitled%2019.avif)

此外，高级 选项卡中还有更多设置项，具体可以参考下别人的教程。

### HTTPS 设置

原则上不推荐直接将 Web UI 反代到公网，自用推荐使用 Tailscale 组网后，使用提供的 overlay IP 访问。下面介绍下如何配置 https 访问。

1. 关闭 启用 Host header 属性验证
    
    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/08/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qBittorrent%20%28DSM%207.2%29/Untitled%2020.avif)
    
2. 按照[这篇教程](https://blog.mitsea.com/3a0b40567ffe42e999eade2fdaf775b0/)设置反代

> Photo by [Philip Oroni](https://unsplash.com/@philipsfuture?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/photos/a-close-up-of-a-black-and-blue-background-mb_VqCIOTiU?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
  