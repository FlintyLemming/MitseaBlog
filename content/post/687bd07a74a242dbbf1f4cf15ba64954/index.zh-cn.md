+++
author = "FlintyLemming"
title = "TrueNAS Core 安装 Debian 虚拟机踩坑"
slug = "687bd07a74a242dbbf1f4cf15ba64954"
date = "2024-01-06"
description = "可能真的没有人闲着无聊会用 TrueNAS Core 的虚拟机吧"
categories = ["Linux"]
tags = ["TrueNAS", "虚拟机"]
image = "https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2024/01/TrueNAS%20Core%20%E5%AE%89%E8%A3%85%20Debian%20%E8%99%9A%E6%8B%9F%E6%9C%BA%E8%B8%A9%E5%9D%91/josh-withers-7TjLFBDOoNE-unsplash.avif"
+++

最新闲着没事干在 TrueNAS Core 上装虚拟机跑点东西，没想到刚开始装 Debian 就遇到坑，这里记录一下问题和解决方案。不过这些问题在 Scale 上是没有的。

## 显示问题

### 问题

默认情况下，VNC 的显示会花屏，导致无法进行安装

![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2024/01/TrueNAS%20Core%20%E5%AE%89%E8%A3%85%20Debian%20%E8%99%9A%E6%8B%9F%E6%9C%BA%E8%B8%A9%E5%9D%91/8cebabe05a4983616eb813e9bcb78bda.avif)

### 解决方案

实际上是分辨率问题，需要在设备里找到 VNC，然后把它的分辨率改成 800x600

![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2024/01/TrueNAS%20Core%20%E5%AE%89%E8%A3%85%20Debian%20%E8%99%9A%E6%8B%9F%E6%9C%BA%E8%B8%A9%E5%9D%91/CleanShot_2024-01-06_at_21.53.392x.avif)

这样就可以正常显示了

![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2024/01/TrueNAS%20Core%20%E5%AE%89%E8%A3%85%20Debian%20%E8%99%9A%E6%8B%9F%E6%9C%BA%E8%B8%A9%E5%9D%91/Untitled.avif)

## 引导问题

### 问题

安装完后，删除 CD-ROM 设备后会发现并没有办法正常重启，提示找不到 UEFI 设备

![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2024/01/TrueNAS%20Core%20%E5%AE%89%E8%A3%85%20Debian%20%E8%99%9A%E6%8B%9F%E6%9C%BA%E8%B8%A9%E5%9D%91/CleanShot_2024-01-06_at_21.36.212x.avif)

刚玩过龙芯 3A6000 的我第一时间就怀疑这个虚拟机的 UEFI 没有往 \EFI\debian 下找引导文件，手动尝试打开 \EFI\debian\grubx64.efi 文件确认

![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2024/01/TrueNAS%20Core%20%E5%AE%89%E8%A3%85%20Debian%20%E8%99%9A%E6%8B%9F%E6%9C%BA%E8%B8%A9%E5%9D%91/CleanShot_2024-01-06_at_21.39.082x.avif)

发现确实可以启动

![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2024/01/TrueNAS%20Core%20%E5%AE%89%E8%A3%85%20Debian%20%E8%99%9A%E6%8B%9F%E6%9C%BA%E8%B8%A9%E5%9D%91/7aa62849c44d159df8c64cea4d1908df.avif)

尝试在 UEFI Shell 里通过下面的命令添加启动项

```bash
bcfg boot add 0 FS0:\EFI\debian\grubx64.efi "Debian”
```

![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2024/01/TrueNAS%20Core%20%E5%AE%89%E8%A3%85%20Debian%20%E8%99%9A%E6%8B%9F%E6%9C%BA%E8%B8%A9%E5%9D%91/CleanShot_2024-01-06_at_21.46.152x.avif)

但是重启后又没了，看起来他这个虚拟机的 EFI 并没有持久化

### 解决方案

从 UEFI 下手不行，只能自适应一下了。在 Debian 系统里把引导文件移到 /EFI/BOOT 里，然后再改一个通用一点的名称，比如 bootx64.efi 试试。

```bash
mkdir /boot/efi/EFI/BOOT
cp /boot/efi/EFI/debian/grubx64.efi /boot/efi/EFI/BOOT/bootx64.efi
```

问题解决，难绷

> Photo by [Josh Withers](https://unsplash.com/@joshwithers?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/a-body-of-water-surrounded-by-snow-covered-mountains-7TjLFBDOoNE?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)
  