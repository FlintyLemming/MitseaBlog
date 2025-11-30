+++
author = "FlintyLemming"
title = "Linux Nvidia 驱动从 runfile 迁移到 apt 管理"
slug = "v378jL9BNFY7u9kczfV9Gx"
date = "2025-11-30"
description = "用起来感觉还行，目前还没遇到什么坑"
categories = ["Linux"]
tags = ["Nvidia", "CUDA"]
image = "https://assets.mitsea.cn/blog/posts/2025/11/Linux%20Nvidia%20%E9%A9%B1%E5%8A%A8%E4%BB%8E%20runfile%20%E8%BF%81%E7%A7%BB%E5%88%B0%20apt%20%E7%AE%A1%E7%90%86/shubham-dhage-xwroH2gD8uw-unsplash.avif"
+++

## 背景

以前在 Linux 上安装 Nvidia 驱动和 CUDA，一般都是直接去官网下一个 CUDA runfile 安装文件，直接运行就给你把驱动和 CUDA 一起安装好了。然后 Nvidia 现在好像是在推用 apt 管理驱动，他们自己弄了个镜像源，里面有驱动和各种安装包。这篇文章就是记录下迁移到 apt 管理驱动的过程。总的来说就是按照官方下面这个文档安装

[https://docs.nvidia.com/datacenter/tesla/driver-installation-guide/ubuntu.html](https://docs.nvidia.com/datacenter/tesla/driver-installation-guide/ubuntu.html "https://docs.nvidia.com/datacenter/tesla/driver-installation-guide/ubuntu.html")

## 卸载驱动和CUDA

1. 执行下面的命令卸载驱动
   ```bash 
   sudo /usr/bin/nvidia-uninstall
   ```

   卸载时可能会出现类似下面的提示，不影响，直接 OK

   ![](https://assets.mitsea.cn/blog/posts/2025/11/Linux%20Nvidia%20%E9%A9%B1%E5%8A%A8%E4%BB%8E%20runfile%20%E8%BF%81%E7%A7%BB%E5%88%B0%20apt%20%E7%AE%A1%E7%90%86/image_xhsow37iET.avif)

   ![](https://assets.mitsea.cn/blog/posts/2025/11/Linux%20Nvidia%20%E9%A9%B1%E5%8A%A8%E4%BB%8E%20runfile%20%E8%BF%81%E7%A7%BB%E5%88%B0%20apt%20%E7%AE%A1%E7%90%86/image_R40PWK5ZXq.avif)

   ![](https://assets.mitsea.cn/blog/posts/2025/11/Linux%20Nvidia%20%E9%A9%B1%E5%8A%A8%E4%BB%8E%20runfile%20%E8%BF%81%E7%A7%BB%E5%88%B0%20apt%20%E7%AE%A1%E7%90%86/image_0PB33nSStI.avif)
2. 执行下面的命令卸载 CUDA
   ```bash 
   sudo /usr/local/cuda/bin/cuda-uninstaller
   ```

   ![](https://assets.mitsea.cn/blog/posts/2025/11/Linux%20Nvidia%20%E9%A9%B1%E5%8A%A8%E4%BB%8E%20runfile%20%E8%BF%81%E7%A7%BB%E5%88%B0%20apt%20%E7%AE%A1%E7%90%86/image_4deVQXwQvo.avif)
   ```bash 
   sudo rm -rf /usr/local/cuda*
   ```

3. 重启服务器

## 设置 Nvidia 官方源

```bash 
echo "deb [signed-by=/usr/share/keyrings/cuda-archive-keyring.gpg] https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/ /" | sudo tee /etc/apt/sources.list.d/cuda-ubuntu2204-x86_64.list

```


重新执行 apt update 后，可以看到上面是原来安装 nvidia container toolkit 添加的源，下面的是驱动和CUDA的源

![](https://assets.mitsea.cn/blog/posts/2025/11/Linux%20Nvidia%20%E9%A9%B1%E5%8A%A8%E4%BB%8E%20runfile%20%E8%BF%81%E7%A7%BB%E5%88%B0%20apt%20%E7%AE%A1%E7%90%86/image_j2--CsAKRe.avif)

然后上面这个源其实可以不要了，因为下面的已经包含了 nvidia container toolkit 的包了，所以我们可以直接删掉他

```bash 
sudo rm -rf /etc/apt/sources.list.d/nvidia-container-toolkit.list
sudo apt update
```


## 安装驱动

一般就直接安装 cuda 就会连带驱动一起安装好

```bash 
sudo apt install cuda
```


![](https://assets.mitsea.cn/blog/posts/2025/11/Linux%20Nvidia%20%E9%A9%B1%E5%8A%A8%E4%BB%8E%20runfile%20%E8%BF%81%E7%A7%BB%E5%88%B0%20apt%20%E7%AE%A1%E7%90%86/image_kfoCjWwhyH.avif)

如果需要锁驱动版本等可以参考官方文档，有一个` nvidia-driver-pinning-<version>`，不需要自己去 hold 包

## 安装 NVIDIA Container Toolkit

直接执行

```bash 
sudo apt-get install -y \
      nvidia-container-toolkit \
      nvidia-container-toolkit-base \
      libnvidia-container-tools \
      libnvidia-container1
```


然后重启 docker 服务

```bash 
sudo systemctl restart docker
```


## 对于 SXM 显卡

以往使用 .run 的安装方式，一般是再补装一个 ubuntu 官方的 `nvidia-fabricmanager-<version>` 包。但是我们加了这个 Nvidia 的源后，就直接安装就行，不需要指定版本了。

```bash 
sudo apt install nvidia-fabricmanager
```


注意这个就不需要指定版本了，因为 `nvidia-fabricmanager` 和 `nvidia-fabricmanager-<version>` 是两个东西。前者是 Nvidia 源里的，后者是 Ubuntu 官方源里的。

![](https://assets.mitsea.cn/blog/posts/2025/11/Linux%20Nvidia%20%E9%A9%B1%E5%8A%A8%E4%BB%8E%20runfile%20%E8%BF%81%E7%A7%BB%E5%88%B0%20apt%20%E7%AE%A1%E7%90%86/image_lOi7beF_0M.avif)
