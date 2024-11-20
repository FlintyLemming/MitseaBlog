+++
author = "FlintyLemming"
title = "使用 Live Share 合作编辑、调试并运行代码"
slug = "17c3a998d242474d929088b299a9a583"
date = "2020-03-11"
description = ""
categories = ["Coding"]
tags = ["协同工作"]
image = "https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2020/03/%E4%BD%BF%E7%94%A8%20Live%20Share%20%E5%90%88%E4%BD%9C%E7%BC%96%E8%BE%91%E3%80%81%E8%B0%83%E8%AF%95%E5%B9%B6%E8%BF%90%E8%A1%8C%E4%BB%A3%E7%A0%81/title.avif"
+++

## 功能演示

[Visual Studio 实时共享 | Visual Studio - Visual Studio](https://visualstudio.microsoft.com/zh-hans/services/live-share/)

简单来说，你可以和别人：

共享代码编辑，并查看对方的编辑位置，避免通过截图交流的尴尬

共同调试，多人调试相同代码，快速查找到问题

共同查看成果，IIS Express 的网页也会在每个人的电脑本地打开

## 配置步骤

### Windows 开发环境配置

1. 安装最新版的 Visual Studio（我这边是 2019 16.4.6）并在 Visual Studio 里登录自己的微软账号
2. 点击右上角的 Live Share。第一次使用，会弹出防火墙提示，请打开。

    ![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2020/03/%E4%BD%BF%E7%94%A8%20Live%20Share%20%E5%90%88%E4%BD%9C%E7%BC%96%E8%BE%91%E3%80%81%E8%B0%83%E8%AF%95%E5%B9%B6%E8%BF%90%E8%A1%8C%E4%BB%A3%E7%A0%81/1.avif)

3. 第一次启动可能会比较慢，稍等片刻，启动完成则会看到这条提示

    ![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2020/03/%E4%BD%BF%E7%94%A8%20Live%20Share%20%E5%90%88%E4%BD%9C%E7%BC%96%E8%BE%91%E3%80%81%E8%B0%83%E8%AF%95%E5%B9%B6%E8%BF%90%E8%A1%8C%E4%BB%A3%E7%A0%81/2.avif)

4. 复制共享链接，或是可以点开详细信息获取功能描述

    ![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2020/03/%E4%BD%BF%E7%94%A8%20Live%20Share%20%E5%90%88%E4%BD%9C%E7%BC%96%E8%BE%91%E3%80%81%E8%B0%83%E8%AF%95%E5%B9%B6%E8%BF%90%E8%A1%8C%E4%BB%A3%E7%A0%81/3.avif)

### macOS 环境配置

1. 安装 Visual Studio Code，然后安装 Live Share 插件

    ![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2020/03/%E4%BD%BF%E7%94%A8%20Live%20Share%20%E5%90%88%E4%BD%9C%E7%BC%96%E8%BE%91%E3%80%81%E8%B0%83%E8%AF%95%E5%B9%B6%E8%BF%90%E8%A1%8C%E4%BB%A3%E7%A0%81/4.avif)

2. 安装完毕后在左侧功能区找到 Live Share 并打开，点击这里，填写共享地址。输入后回车即可连接。

    ![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2020/03/%E4%BD%BF%E7%94%A8%20Live%20Share%20%E5%90%88%E4%BD%9C%E7%BC%96%E8%BE%91%E3%80%81%E8%B0%83%E8%AF%95%E5%B9%B6%E8%BF%90%E8%A1%8C%E4%BB%A3%E7%A0%81/5.avif)

3. 连接成功后，即可看到项目目录

    ![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2020/03/%E4%BD%BF%E7%94%A8%20Live%20Share%20%E5%90%88%E4%BD%9C%E7%BC%96%E8%BE%91%E3%80%81%E8%B0%83%E8%AF%95%E5%B9%B6%E8%BF%90%E8%A1%8C%E4%BB%A3%E7%A0%81/6.avif)

4. 在 Visual Studio 里点击运行，macOS 上的 Visual Studio Code 同样会进入 debug 状态。并且，IIS Express 的网页也会共享过来，非常方便。

    ![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2020/03/%E4%BD%BF%E7%94%A8%20Live%20Share%20%E5%90%88%E4%BD%9C%E7%BC%96%E8%BE%91%E3%80%81%E8%B0%83%E8%AF%95%E5%B9%B6%E8%BF%90%E8%A1%8C%E4%BB%A3%E7%A0%81/7.avif)

    ![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2020/03/%E4%BD%BF%E7%94%A8%20Live%20Share%20%E5%90%88%E4%BD%9C%E7%BC%96%E8%BE%91%E3%80%81%E8%B0%83%E8%AF%95%E5%B9%B6%E8%BF%90%E8%A1%8C%E4%BB%A3%E7%A0%81/8.avif)

## 其他设置

### 基本权限设置

若要在 Visual Studio Code 中也能开始编译并运行，需要进行权限设置，否则会提示如下错误

![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2020/03/%E4%BD%BF%E7%94%A8%20Live%20Share%20%E5%90%88%E4%BD%9C%E7%BC%96%E8%BE%91%E3%80%81%E8%B0%83%E8%AF%95%E5%B9%B6%E8%BF%90%E8%A1%8C%E4%BB%A3%E7%A0%81/9.avif)

只需要在 Visual Studio 的选项中找到 Live Share - 常规，将图中两个位置改为 True 即可

![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2020/03/%E4%BD%BF%E7%94%A8%20Live%20Share%20%E5%90%88%E4%BD%9C%E7%BC%96%E8%BE%91%E3%80%81%E8%B0%83%E8%AF%95%E5%B9%B6%E8%BF%90%E8%A1%8C%E4%BB%A3%E7%A0%81/10.avif)

### 网络设置

建立连接通过香港的微软服务器握手，如果主端没有公网 IP，则会全程通过微软服务器转发内容，速度可能会比较慢。但如果有公网 IP，请打开 5990 端口，并建议打开后面的端口例如 5991、5992等等，因为如果 5990 被占用，则会顺延。同时也建议打开路由器的 UPnP 功能。