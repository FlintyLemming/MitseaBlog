+++
author = "FlintyLemming"
title = "群晖 SMB 多通道官方更新观察"
slug = "6d750e1c5eb7452490d1476e9d73ab45"
date = "2023-01-12"
categories = ["HomeLab", "Network"]
tags = ["SMB"]
image = "https://img.mitsea.com/blog/posts/2023/01/%E7%BE%A4%E6%99%96%20SMB%20%E5%A4%9A%E9%80%9A%E9%81%93%E5%AE%98%E6%96%B9%E6%9B%B4%E6%96%B0%E8%A7%82%E5%AF%9F/shubham-dhage-GG0IQLqOLyA-unsplash.jpg?x-oss-process=style/ImageCompress"
+++

群晖最近更新了 SMB Server，官方支持了 SMB 多通道，原先只能通过修改 SMB config 文件启用，更新系统还会失效，不是很方便。这对于有两个 1GbE 口的群晖还是比较实用的。

![](https://img.mitsea.com/blog/posts/2023/01/%E7%BE%A4%E6%99%96%20SMB%20%E5%A4%9A%E9%80%9A%E9%81%93%E5%AE%98%E6%96%B9%E6%9B%B4%E6%96%B0%E8%A7%82%E5%AF%9FUntitled.png?x-oss-process=style/ImageCompress)

## 准备工作

### 硬件配置

如果你的环境是千兆，那你的 PC 需要有至少两个网口，然后按照下图连接网线

![](https://img.mitsea.com/blog/posts/2023/01/%E7%BE%A4%E6%99%96%20SMB%20%E5%A4%9A%E9%80%9A%E9%81%93%E5%AE%98%E6%96%B9%E6%9B%B4%E6%96%B0%E8%A7%82%E5%AF%9F/Untitled.png?x-oss-process=style/ImageCompress)

如果环境是 2.5G 或者 10G，电脑就不需要插两根网线了

![](https://img.mitsea.com/blog/posts/2023/01/%E7%BE%A4%E6%99%96%20SMB%20%E5%A4%9A%E9%80%9A%E9%81%93%E5%AE%98%E6%96%B9%E6%9B%B4%E6%96%B0%E8%A7%82%E5%AF%9FUntitled%202.png?x-oss-process=style/ImageCompress)

当然如果有多个交换机，下面这样也是可以的，我就是这么用的

![](https://img.mitsea.com/blog/posts/2023/01/%E7%BE%A4%E6%99%96%20SMB%20%E5%A4%9A%E9%80%9A%E9%81%93%E5%AE%98%E6%96%B9%E6%9B%B4%E6%96%B0%E8%A7%82%E5%AF%9FUntitled%203.png?x-oss-process=style/ImageCompress)

### 软件配置

群晖在控制面板 - SMB选项卡 - 高级设置 - 其他选项卡 - 启用SMB3多通道

![](https://img.mitsea.com/blog/posts/2023/01/%E7%BE%A4%E6%99%96%20SMB%20%E5%A4%9A%E9%80%9A%E9%81%93%E5%AE%98%E6%96%B9%E6%9B%B4%E6%96%B0%E8%A7%82%E5%AF%9FUntitled%204.png?x-oss-process=style/ImageCompress)

Windows 应该不用设置，默认是开启的。可以通过 `Get-SmbServerConfiguration` 在 PowerShell 里检查一下是否开启。如果没开启，可以通过 `Set-SmbServerConfiguration -EnableMultiChannel 1` 开启。

## 测试

### 上传

大概 210MB/s 左右

![](https://img.mitsea.com/blog/posts/2023/01/%E7%BE%A4%E6%99%96%20SMB%20%E5%A4%9A%E9%80%9A%E9%81%93%E5%AE%98%E6%96%B9%E6%9B%B4%E6%96%B0%E8%A7%82%E5%AF%9FUntitled%205.png?x-oss-process=style/ImageCompress)

### 下载

大概 210MB/s 左右

![](https://img.mitsea.com/blog/posts/2023/01/%E7%BE%A4%E6%99%96%20SMB%20%E5%A4%9A%E9%80%9A%E9%81%93%E5%AE%98%E6%96%B9%E6%9B%B4%E6%96%B0%E8%A7%82%E5%AF%9FUntitled%206.png?x-oss-process=style/ImageCompress)

### 大量中等文件混合下载

有正常的波动，总体比 1Gbps 快不少

![](https://img.mitsea.com/blog/posts/2023/01/%E7%BE%A4%E6%99%96%20SMB%20%E5%A4%9A%E9%80%9A%E9%81%93%E5%AE%98%E6%96%B9%E6%9B%B4%E6%96%B0%E8%A7%82%E5%AF%9FUntitled%207.png?x-oss-process=style/ImageCompress)

## 其他

细心的应该观察到了，我下载时使用的是 Directory Opus，上传时使用的是自带 Explorer。这是因为如果下载使用 Explorer，上传使用 DO，速度都只有千兆。

![](https://img.mitsea.com/blog/posts/2023/01/%E7%BE%A4%E6%99%96%20SMB%20%E5%A4%9A%E9%80%9A%E9%81%93%E5%AE%98%E6%96%B9%E6%9B%B4%E6%96%B0%E8%A7%82%E5%AF%9FUntitled%208.png?x-oss-process=style/ImageCompress)

可能是目前还有兼容性问题和bug，不过感觉问题不大。
