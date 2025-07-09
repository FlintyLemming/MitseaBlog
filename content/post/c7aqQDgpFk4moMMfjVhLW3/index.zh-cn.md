+++
author = "FlintyLemming"
title = "QNAP TS-453B mini 安装黑群晖硬盘识别问题解决"
slug = "c7aqQDgpFk4moMMfjVhLW3"
date = "2025-01-01"
description = ""
categories = ["HomeLab"]
tags = ["黑群晖"]
image = "https://img.mitsea.com/blog/posts/2025/01/QNAP%20TS-453B%20mini%20%E5%AE%89%E8%A3%85%E9%BB%91%E7%BE%A4%E6%99%96%E7%A1%AC%E7%9B%98%E8%AF%86%E5%88%AB%E9%97%AE%E9%A2%98%E8%A7%A3%E5%86%B3/zach-lezniewicz-E4IDq9NfFYY-unsplash.avif"
+++

这台机器用的外挂 SATA 控制器不能被大多数 Linux 系统默认识别，Windows 倒是可以，所以黑群晖自然也是识别不到。

解决方案参考知乎这篇文章修改 GRUB，既然改的是引导，所以其实跟你用什么群晖机型无关，它在 rr 的 bootloader 里执行 `lsblk` 就看不到设备

[https://zhuanlan.zhihu.com/p/409567877](https://zhuanlan.zhihu.com/p/409567877)

解决方法他这里也写了，就是改 GRUB 然后修改 `GRUB_CMDLINE_LINUX="pcie_port_pm=off"` ，只不过 rr 的引导是靠 grub.cfg 来定义的 GRUB，所以刚上手不知道这个文件对应改哪里。摸索了一下发现改 `set RR_CMDLINE` 内的内容有效。

在这行的最后加一段 `pcie_port_pm=off` 就可以了

![](https://img.mitsea.com/blog/posts/2025/01/QNAP%20TS-453B%20mini%20%E5%AE%89%E8%A3%85%E9%BB%91%E7%BE%A4%E6%99%96%E7%A1%AC%E7%9B%98%E8%AF%86%E5%88%AB%E9%97%AE%E9%A2%98%E8%A7%A3%E5%86%B3/image_850z0ohy1Z.avif)

> Photo by [Zach Lezniewicz](https://unsplash.com/@zachlez?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/a-black-and-white-photo-of-a-large-body-of-water-E4IDq9NfFYY?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)
      