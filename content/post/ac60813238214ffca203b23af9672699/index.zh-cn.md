+++
author = "FlintyLemming"
title = "X410 使用方法"
slug = "ac60813238214ffca203b23af9672699"
date = "2021-03-25"
description = ""
categories = ["Linux", "Windows"]
tags = ["Windows", "Linux"]
image = "https://assets.mitsea.cn/blog/posts/2021/03/X410%20%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95/title.avif"
+++

之前对 Linux Xserver 不了解，所以买了 X410 一直不会用。今天从官网了解了下使用方法，下面记一下如何搭配 WSL 使用。

1. 安装并启动 WSL，我这边使用的是 Ubuntu
2. 更新软件源列表（要换源的可以换源，我用 ustc 的没有问题）
    
    ```
    sudo apt-get update
    ```
    
3. wsl 的系统默认是没有安装桌面的，我们要先安装一个桌面，这边安装的是 xfce
    
    ```
    sudo apt install xfce4 xfce4-terminal gtk2-engines-pixbuf
    ```
    
4. 打开 X410 准备使用
5. 在 Ubuntu 里继续操作，设置变量并启动 xfce
    
    ```
    export DISPLAY=127.0.0.1:0.0
    xfce4-session
    ```
    
6. 启动成功
    
    ![](https://assets.mitsea.cn/blog/posts/2021/03/X410%20%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95/1.avif)