+++
author = "FlintyLemming"
title = "使用 Active Backup for Business 备份虚拟机（Hyper-V）"
slug = "eec7801069a649c29907229cc29044fb"
date = "2020-02-26"
description = ""
categories = ["HomeLab"]
tags = ["Synology"]
image = "https://img.flinty.moe/blog/posts/2020/02/%E4%BD%BF%E7%94%A8%20Active%20Backup%20for%20Business%20%E5%A4%87%E4%BB%BD%E8%99%9A%E6%8B%9F%E6%9C%BA%EF%BC%88Hyper-V%EF%BC%89/title.avif"
+++

在生产环境中，虚拟机集群是经常被采用的方式。通过群晖附带的 Active Backup for Business 可以可靠并高效的备份以及还原这些虚拟机，并尽量减少存储开销，这是他最大的亮点。同时，它还可以拿来备份实体机器和服务器，通过下面的基础设置，可以快速使用起这些功能。

## Windows 端配置

### 配置 Windows RM

首先确保 Windows RM（Windows Remote Management）是否已经启用，打开 PowerShell，输入下面的命令查看服务状态

    winrm enumerate winrm/config/listener

如果返回如下内容，则表示没有启用

![](https://img.flinty.moe/blog/posts/2020/02/%E4%BD%BF%E7%94%A8%20Active%20Backup%20for%20Business%20%E5%A4%87%E4%BB%BD%E8%99%9A%E6%8B%9F%E6%9C%BA%EF%BC%88Hyper-V%EF%BC%89/1.avif)

下面以默认配置启用 Windows RM

1. 使用管理员身份运行 PowerShell 

    ![](https://img.flinty.moe/blog/posts/2020/02/%E4%BD%BF%E7%94%A8%20Active%20Backup%20for%20Business%20%E5%A4%87%E4%BB%BD%E8%99%9A%E6%8B%9F%E6%9C%BA%EF%BC%88Hyper-V%EF%BC%89/2.avif)

2. 输入如下命令开始 Windows RM 的配置

        Winrm quickconfig

3. 询问是否启动服务，输入 y，然后按回车

    ![](https://img.flinty.moe/blog/posts/2020/02/%E4%BD%BF%E7%94%A8%20Active%20Backup%20for%20Business%20%E5%A4%87%E4%BB%BD%E8%99%9A%E6%8B%9F%E6%9C%BA%EF%BC%88Hyper-V%EF%BC%89/3.avif)

4. 然后提示启动成功。但如果提示下面的错误，会导致群晖无法通过 http 连接 Windows。请按照下面的方法将网络位置修改为 专用网络。**并再次执行第 2 步的配置操作。**

    ![](https://img.flinty.moe/blog/posts/2020/02/%E4%BD%BF%E7%94%A8%20Active%20Backup%20for%20Business%20%E5%A4%87%E4%BB%BD%E8%99%9A%E6%8B%9F%E6%9C%BA%EF%BC%88Hyper-V%EF%BC%89/4.avif)

5. 输入下面的命令查看服务的连接信息

        winrm e winrm/config/listener

### 配置脚本执行策略

此外，还需要配置 PowerShell 的脚本执行策略，否则将会出现下面的错误

![](https://img.flinty.moe/blog/posts/2020/02/%E4%BD%BF%E7%94%A8%20Active%20Backup%20for%20Business%20%E5%A4%87%E4%BB%BD%E8%99%9A%E6%8B%9F%E6%9C%BA%EF%BC%88Hyper-V%EF%BC%89/5.avif)

1. 查看当前策略，PowerShell 中执行

        get-executionpolicy

2. 默认应该是 Unrestricted，通过执行下面的命令修改为 RemoteSigned

        set-executionpolicy remotesigned

3. 最后查看一下修改状态，确认已经修改完毕

    ![](https://img.flinty.moe/blog/posts/2020/02/%E4%BD%BF%E7%94%A8%20Active%20Backup%20for%20Business%20%E5%A4%87%E4%BB%BD%E8%99%9A%E6%8B%9F%E6%9C%BA%EF%BC%88Hyper-V%EF%BC%89/6.avif)

## DSM 端配置

1. 打开 Active Backup for Business，按照下图打开 **添加 Hypervisor** 窗口

    ![](https://img.flinty.moe/blog/posts/2020/02/%E4%BD%BF%E7%94%A8%20Active%20Backup%20for%20Business%20%E5%A4%87%E4%BB%BD%E8%99%9A%E6%8B%9F%E6%9C%BA%EF%BC%88Hyper-V%EF%BC%89/7.avif)

2. 地址填写 Windows 机器的 IP 地址，账号密码就是 Windows 的登录账号密码。应用后可以查看 Windows 计算机的状态，确认无误后关闭窗口。

    ![](https://img.flinty.moe/blog/posts/2020/02/%E4%BD%BF%E7%94%A8%20Active%20Backup%20for%20Business%20%E5%A4%87%E4%BB%BD%E8%99%9A%E6%8B%9F%E6%9C%BA%EF%BC%88Hyper-V%EF%BC%89/8.avif)

3. 这边就能查询到我的两台 Hyper-V 虚拟机了

    ![](https://img.flinty.moe/blog/posts/2020/02/%E4%BD%BF%E7%94%A8%20Active%20Backup%20for%20Business%20%E5%A4%87%E4%BB%BD%E8%99%9A%E6%8B%9F%E6%9C%BA%EF%BC%88Hyper-V%EF%BC%89/9.avif)

4. 之后，通过点按 创建任务 按钮即可设置备份计划。