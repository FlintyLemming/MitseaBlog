+++
author = "FlintyLemming"
title = "Linux Nvidia GPU 驱动离线更新（runfile）"
slug = "1a57bda595c580088006c17d6ba2a744"
date = "2025-11-30"
description = "主要适用于离线环境"
categories = ["Linux"]
tags = ["Nvidia", "CUDA"]
image = "https://assets.mitsea.cn/blog/posts/2025/11/Linux%20Nvidia%20GPU%20%E9%A9%B1%E5%8A%A8%E7%A6%BB%E7%BA%BF%E6%9B%B4%E6%96%B0%EF%BC%88runfile%EF%BC%89/yuri-krupenin-DkBzb1Qr_Fc-unsplash.avif"
+++

## 事前检查

一般来说驱动都是用的 runfile 安装的，apt 源安装驱动是近期流行的东西，而且需要网络。不过可以通过下面的命令检查

```bash 
ls /usr/bin | grep nvidia-uninstall
```


如果能找到这个文件，那就是 .run file 安装的驱动

## 下载驱动

在这里下载驱动，选择你的系统、版本和 runfile 下载

[https://developer.nvidia.com/cuda-downloads](https://developer.nvidia.com/cuda-downloads "https://developer.nvidia.com/cuda-downloads")

![](https://assets.mitsea.cn/blog/posts/2025/11/Linux%20Nvidia%20GPU%20%E9%A9%B1%E5%8A%A8%E7%A6%BB%E7%BA%BF%E6%9B%B4%E6%96%B0%EF%BC%88runfile%EF%BC%89/image_BkbnkdpCBX.avif)

## 卸载驱动

安装新版驱动前需要先卸载旧版本驱动

```bash 
sudo /usr/bin/nvidia-uninstall
```


卸载时可能会出现类似下面的提示，不影响，直接 OK

![](https://assets.mitsea.cn/blog/posts/2025/11/Linux%20Nvidia%20GPU%20%E9%A9%B1%E5%8A%A8%E7%A6%BB%E7%BA%BF%E6%9B%B4%E6%96%B0%EF%BC%88runfile%EF%BC%89/image_GV9hS65ifz.avif)

![](https://assets.mitsea.cn/blog/posts/2025/11/Linux%20Nvidia%20GPU%20%E9%A9%B1%E5%8A%A8%E7%A6%BB%E7%BA%BF%E6%9B%B4%E6%96%B0%EF%BC%88runfile%EF%BC%89/image_9ShPKUmwXX.avif)

![](https://assets.mitsea.cn/blog/posts/2025/11/Linux%20Nvidia%20GPU%20%E9%A9%B1%E5%8A%A8%E7%A6%BB%E7%BA%BF%E6%9B%B4%E6%96%B0%EF%BC%88runfile%EF%BC%89/image_1TzhGIevk-.avif)

理论上只需要卸载驱动就可以，只要你不切换到 apt 方式安装驱动，就不需要卸载 CUDA，CUDA 可以多版本并存

nvidia-container-toolkit 不需要卸载，更新驱动后还能继续用

卸载后建议重启服务器

## 安装驱动

与先前一样正常安装就可以，runfile 自带新版本 CUDA 和驱动

![](https://assets.mitsea.cn/blog/posts/2025/11/Linux%20Nvidia%20GPU%20%E9%A9%B1%E5%8A%A8%E7%A6%BB%E7%BA%BF%E6%9B%B4%E6%96%B0%EF%BC%88runfile%EF%BC%89/image_IhJsmYmESy.avif)

如果是新显卡，还会有开源和闭源驱动的选项，按照 Nvidia 的策略，建议是选择开源驱动

也可以选择更新 Container Toolkit，在下面这个地址里找到以下几个包并安装（根据自己的机器系统选择）

[https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2404/x86\_64/](https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2404/x86_64/ "https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2404/x86_64/")

```markdown 
nvidia-container-toolkit
nvidia-container-toolkit-base
libnvidia-container-tools
libnvidia-container1
```


## 关于 SXM 显卡

SXM 显卡需要安装 Nvidia Fabric Manager，它需要与驱动版本严格匹配。但是某些时候 runfile 驱动版本号太新，Ubuntu 的 apt 源里还没有新版本的 Nvidia Fabric Manager，此时就需要从 Nvidia 的源里去下载 deb 包手动安装，下载 nvidia-fabricmanager\_ 开头的那个 deb 包。

[https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2404/x86\_64/](https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2404/x86_64/ "https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2404/x86_64/")

> Photo by [Yuri Krupenin](https://unsplash.com/@cubeofwood?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/photos/snow-covered-branches-in-golden-sunset-light-DkBzb1Qr_Fc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
      