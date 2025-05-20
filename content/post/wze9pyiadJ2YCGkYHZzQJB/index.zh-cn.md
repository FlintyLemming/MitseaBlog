+++
author = "FlintyLemming"
title = "TCL-S1916 交换机配置"
slug = "wze9pyiadJ2YCGkYHZzQJB"
date = "2025-05-20"
description = "谁能想到60块16口还能带网管啊"
categories = ["HomeLab"]
tags = ["交换机"]
image = "https://hf-image.mitsea.com:8840/blog/posts/2025/05/TCL-S1916%20%E4%BA%A4%E6%8D%A2%E6%9C%BA%E9%85%8D%E7%BD%AE/squids-z-0WNp7qDtAaI-unsplash.avif"
+++

## 背景

这款 16 口千兆交换机是 60 块钱淘宝捡的，刚到手的时候毛病很多，有的端口不能用，能用的端口之间还是隔离的，有怀疑过是不是网管交换机，但是标签上完全没写

![](https://hf-image.mitsea.com:8840/blog/posts/2025/05/TCL-S1916%20%E4%BA%A4%E6%8D%A2%E6%9C%BA%E9%85%8D%E7%BD%AE/photo_T_gSPgqAwe.avif)

查了下，发现还真是个网管交换机，下面写一下配置方法

## 准备工作

1. 打开[这个地址](https://index.mitsea.com/%E8%BD%AF%E4%BB%B6/%E9%A9%B1%E5%8A%A8%E5%92%8C%E5%85%B6%E4%BB%96%E9%95%9C%E5%83%8F/TCL-S1916%20%E7%BD%91%E7%AE%A1%E4%BA%A4%E6%8D%A2%E6%9C%BA)，我把软件都放进去了，下载后解压 **TCL-S1916网管交换机软件.zip**
2. 安装 WinPcap，管理软件依赖里面的某个 dll
3. 将电脑网口与交换机的 **16 口**连接，也就是最后一个口，交换机只能通过这个口连接
4. 给电脑设置一个固定 IP，任意都行

   ![](https://hf-image.mitsea.com:8840/blog/posts/2025/05/TCL-S1916%20%E4%BA%A4%E6%8D%A2%E6%9C%BA%E9%85%8D%E7%BD%AE/6_bDxKJBMGEu.avif)

## 配置作业

1. 双击打开 S1916Manager.exe，这里选择连接交换机的网卡

   ![](https://hf-image.mitsea.com:8840/blog/posts/2025/05/TCL-S1916%20%E4%BA%A4%E6%8D%A2%E6%9C%BA%E9%85%8D%E7%BD%AE/2_ShG00mNVH6.avif)

2. 连接上交换机后，上面的操作按钮就会亮起，选择 配置操作-读交换机配置信息

   ![](https://hf-image.mitsea.com:8840/blog/posts/2025/05/TCL-S1916%20%E4%BA%A4%E6%8D%A2%E6%9C%BA%E9%85%8D%E7%BD%AE/3_wMx1Odv0dv.avif)

   读取完可能会提示这个错误，好像没影响

   ![](https://hf-image.mitsea.com:8840/blog/posts/2025/05/TCL-S1916%20%E4%BA%A4%E6%8D%A2%E6%9C%BA%E9%85%8D%E7%BD%AE/4_tp7HD354so.avif)

3. 读取完后就可以看到当前交换机的配置了，如果你到手用着有问题，肯定是哪里配置改过，比如我这里 VLAN 配置就会导致我遇到的隔离问题

   ![](https://hf-image.mitsea.com:8840/blog/posts/2025/05/TCL-S1916%20%E4%BA%A4%E6%8D%A2%E6%9C%BA%E9%85%8D%E7%BD%AE/1_l4SEuP3t3c.avif)

4. 你可以自己修改设置，然后写回配置。如果只是当做普通交换机用的话，直接选择 系统操作-恢复出厂设置 即可。点击后会自动写回配置，不需要重新写一次配置。

   ![](https://hf-image.mitsea.com:8840/blog/posts/2025/05/TCL-S1916%20%E4%BA%A4%E6%8D%A2%E6%9C%BA%E9%85%8D%E7%BD%AE/5_mcBx6NVGgI.avif)

至此就把这个交换机恢复为一个普通交换机的状态了

> Photo by [Squids Z](https://unsplash.com/@squids93?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on[Unsplash](https://unsplash.com/photos/abstract-building-with-orange-and-pink-hues-0WNp7qDtAaI?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)
      