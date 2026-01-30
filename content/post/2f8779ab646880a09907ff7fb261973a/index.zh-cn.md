+++
author = "FlintyLemming"
title = "B650 南桥独立扩展卡简测（开发中）"
slug = "2f8779ab646880a09907ff7fb261973a"
date = "2026-01-30"
description = "还在开发中，所以有一些问题"
categories = ["HomeLab"]
tags = ["南桥"]
image = "https://assets.mitsea.cn/blog/posts/2026/01/B650%20%E5%8D%97%E6%A1%A5%E7%8B%AC%E7%AB%8B%E6%89%A9%E5%B1%95%E5%8D%A1%E7%AE%80%E6%B5%8B%EF%BC%88%E5%BC%80%E5%8F%91%E4%B8%AD%EF%BC%89/ruben-mavarez-yi0RrmWvd-8-unsplash.avif"
+++

嘉立创上之前有人开源了个 B650 南桥扩展卡，群友做了一版，拿来测测

https://oshwhub.com/wesd/b650

外观如下，两个 nvme m.2，四个 SATA，一个 USB-E

![](https://assets.mitsea.cn/blog/posts/2026/01/B650%20%E5%8D%97%E6%A1%A5%E7%8B%AC%E7%AB%8B%E6%89%A9%E5%B1%95%E5%8D%A1%E7%AE%80%E6%B5%8B%EF%BC%88%E5%BC%80%E5%8F%91%E4%B8%AD%EF%BC%89/20260130_092339.avif)

测试平台为 Radxa O6，CIX P1 的平台。目前 M.2_1 还不能使用，插上硬盘会卡自检，PCIe 协商速度从 Gen 2 到 Gen 4 都不行。x86 平台基本上好像没问题。

所以目前只能这么用

![](https://assets.mitsea.cn/blog/posts/2026/01/B650%20%E5%8D%97%E6%A1%A5%E7%8B%AC%E7%AB%8B%E6%89%A9%E5%B1%95%E5%8D%A1%E7%AE%80%E6%B5%8B%EF%BC%88%E5%BC%80%E5%8F%91%E4%B8%AD%EF%BC%89/20260130_103755.avif)

系统内可以正常识别南桥芯片，硬盘也可以正常识别

![](https://assets.mitsea.cn/blog/posts/2026/01/B650%20%E5%8D%97%E6%A1%A5%E7%8B%AC%E7%AB%8B%E6%89%A9%E5%B1%95%E5%8D%A1%E7%AE%80%E6%B5%8B%EF%BC%88%E5%BC%80%E5%8F%91%E4%B8%AD%EF%BC%89/iShot_2026-01-30_09.51.16.avif)

装个 fnOS，硬盘也可以正常识别并使用

![](https://assets.mitsea.cn/blog/posts/2026/01/B650%20%E5%8D%97%E6%A1%A5%E7%8B%AC%E7%AB%8B%E6%89%A9%E5%B1%95%E5%8D%A1%E7%AE%80%E6%B5%8B%EF%BC%88%E5%BC%80%E5%8F%91%E4%B8%AD%EF%BC%89/iShot_2026-01-30_10.44.44.avif)

> Photo by [Ruben Mavarez](https://unsplash.com/@justalifein?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/photos/soft-pink-flowers-with-green-leaves-in-soft-focus-yi0RrmWvd-8?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
      