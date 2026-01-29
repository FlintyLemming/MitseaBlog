+++
author = "FlintyLemming"
title = "Windows Linux 子系统（wsl）快速上手"
slug = "2f4779ab6468806c9eb2fc419f0b683c"
date = "2026-01-26"
description = "参考指南"
categories = ["Linux", "Microsoft"]
tags = ["wsl"]
image = "https://assets.mitsea.cn/blog/posts/2026/01/Windows%20Linux%20%E5%AD%90%E7%B3%BB%E7%BB%9F%EF%BC%88wsl%EF%BC%89%E5%BF%AB%E9%80%9F%E4%B8%8A%E6%89%8B/mike-hindle-a8Slhd4Kvxw-unsplash.avif"
+++

本篇文章旨在帮助用户快速上手 wsl，所有步骤在 Windows 11 24H2 和更新版本上有效，老系统未经验证

## 安装

1. 右键开始菜单，打开终端管理员，执行
    
    ```
    wsl --install
    ```
    
    安装后需要重启电脑
    
    ![](https://assets.mitsea.cn/blog/posts/2026/01/Windows%20Linux%20%E5%AD%90%E7%B3%BB%E7%BB%9F%EF%BC%88wsl%EF%BC%89%E5%BF%AB%E9%80%9F%E4%B8%8A%E6%89%8B/image.avif)
    
2. 重启后，安装一个 Linux 发行版，这里选择 Ubuntu 24.04
    
    ```
    wsl --install Ubuntu-24.04
    ```
    
    安装后会引导你设置 Ubuntu 的账号密码，完成后就可以用 Ubuntu 了
    
    ![](https://assets.mitsea.cn/blog/posts/2026/01/Windows%20Linux%20%E5%AD%90%E7%B3%BB%E7%BB%9F%EF%BC%88wsl%EF%BC%89%E5%BF%AB%E9%80%9F%E4%B8%8A%E6%89%8B/image%201.avif)
    

## 打开 wsl

有几种方法可以打开 wsl

### 直接打开

打开终端后，点击加号旁边的下拉列表后，可以看到安装的 wsl 子系统

![](https://assets.mitsea.cn/blog/posts/2026/01/Windows%20Linux%20%E5%AD%90%E7%B3%BB%E7%BB%9F%EF%BC%88wsl%EF%BC%89%E5%BF%AB%E9%80%9F%E4%B8%8A%E6%89%8B/image%202.avif)

这样就是默认进入到 Linux 系统的 home 目录下，跟用 Linux 终端没区别

![](https://assets.mitsea.cn/blog/posts/2026/01/Windows%20Linux%20%E5%AD%90%E7%B3%BB%E7%BB%9F%EF%BC%88wsl%EF%BC%89%E5%BF%AB%E9%80%9F%E4%B8%8A%E6%89%8B/image%203.avif)

### 在项目文件夹下启动 wsl

在空白处右键打开终端，然后输入 `wsl` 即可快速进入到这个文件夹开始操作

![](https://assets.mitsea.cn/blog/posts/2026/01/Windows%20Linux%20%E5%AD%90%E7%B3%BB%E7%BB%9F%EF%BC%88wsl%EF%BC%89%E5%BF%AB%E9%80%9F%E4%B8%8A%E6%89%8B/image%204.avif)

## 使用 wsl

### 文件互通

如上所示，wsl 与 Windows 主系统文件互通，例如 Windows 下的 `C:\Users\mucr\Projects\clawdbot` 在 wsl 里对应的是 `/mnt/c/Users/mucr/Projects/clawdbot`

### Docker 互通

Windows 下的 Rancher Desktop 可以直接与 wsl 互联，可以互相管理

![](https://assets.mitsea.cn/blog/posts/2026/01/Windows%20Linux%20%E5%AD%90%E7%B3%BB%E7%BB%9F%EF%BC%88wsl%EF%BC%89%E5%BF%AB%E9%80%9F%E4%B8%8A%E6%89%8B/image%205.avif)

> Photo by [Mike Hindle](https://unsplash.com/@mikehindle?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/photos/modern-building-facade-with-repeating-vertical-elements-a8Slhd4Kvxw?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
      