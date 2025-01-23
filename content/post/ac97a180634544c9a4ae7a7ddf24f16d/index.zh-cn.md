+++
author = "FlintyLemming"
title = "群晖 DS1621+ 入手与配置"
slug = "ac97a180634544c9a4ae7a7ddf24f16d"
date = "2023-03-11"
description = ""
categories = ["Consumer"]
tags = ["NAS"]
image = "https://image.mitsea.com/blog/posts/2023/03/%E7%BE%A4%E6%99%96%20DS1621%2B%20%E5%85%A5%E6%89%8B%E4%B8%8E%E9%85%8D%E7%BD%AE/tom-mrazek-sR8_Bi6VUJM-unsplash.avif"
+++

由于群晖 2023 年款挤牙膏，1823xs+ 用的还是 Ryzen V1780B 与 V1500B 几乎无异，所以我估摸着就算出 1623+ 性能也不咋地，就直接买了 1621+。下面简单介绍下我周边硬件的选择和配置

## 硬件一览

| 设备类型 | 型号 | 价格 |
| --- | --- | --- |
| NAS 本体 | DS1621+ | 6000 |
| 机械硬盘 | 4块 东芝 MG07ACA12TE 12T 拆机盘 | 2640 |
| 2.5寸 固态硬盘 | 2块 闪迪云盘ECO 1.92T 拆机盘 | 1200 |
| nvme 固态硬盘 | 2块 西数 SN730 1T 二手 | 660 |
| 网卡 | 浪潮 X710 | 370 |
| 内存 | 2根 DDR4 SODIMM 16G 2400MHz | 320 |
| 总计 |  | 11190 |

![](https://image.mitsea.com/blog/posts/2023/03/%E7%BE%A4%E6%99%96%20DS1621%2B%20%E5%85%A5%E6%89%8B%E4%B8%8E%E9%85%8D%E7%BD%AE/Untitled.avif)

## 存储池分配

两块 SATA SSD 做缓存盘，因为我的套件默认全部安装在机械硬盘存储池中，所以使用大容量 SSD 作为缓存盘可以极大提高速度

![](https://image.mitsea.com/blog/posts/2023/03/%E7%BE%A4%E6%99%96%20DS1621%2B%20%E5%85%A5%E6%89%8B%E4%B8%8E%E9%85%8D%E7%BD%AE/Untitled%201.avif)

两块 nvme SSD 通过命令行做成 Raid0 存储空间，作为临时高速存储。用来作为下载、刷pt和临时中转文件。

![](https://image.mitsea.com/blog/posts/2023/03/%E7%BE%A4%E6%99%96%20DS1621%2B%20%E5%85%A5%E6%89%8B%E4%B8%8E%E9%85%8D%E7%BD%AE/Untitled%202.avif)

## 简单测试

### SMB 下载

![](https://image.mitsea.com/blog/posts/2023/03/%E7%BE%A4%E6%99%96%20DS1621%2B%20%E5%85%A5%E6%89%8B%E4%B8%8E%E9%85%8D%E7%BD%AE/Untitled%203.avif)

### SMB 上传

![](https://image.mitsea.com/blog/posts/2023/03/%E7%BE%A4%E6%99%96%20DS1621%2B%20%E5%85%A5%E6%89%8B%E4%B8%8E%E9%85%8D%E7%BD%AE/9468317cee88dee6cea77ccdfc73d876.avif)

### http 上传下载

![](https://image.mitsea.com/blog/posts/2023/03/%E7%BE%A4%E6%99%96%20DS1621%2B%20%E5%85%A5%E6%89%8B%E4%B8%8E%E9%85%8D%E7%BD%AE/Untitled%204.avif)

## 硬件详情

### NAS 本体

![](https://image.mitsea.com/blog/posts/2023/03/%E7%BE%A4%E6%99%96%20DS1621%2B%20%E5%85%A5%E6%89%8B%E4%B8%8E%E9%85%8D%E7%BD%AE/IMG_0430.avif)

![](https://image.mitsea.com/blog/posts/2023/03/%E7%BE%A4%E6%99%96%20DS1621%2B%20%E5%85%A5%E6%89%8B%E4%B8%8E%E9%85%8D%E7%BD%AE/IMG_0431.avif)

### 内存条

虽然原装是 ECC 内存，但实际上也是可以用普通 DDR4 内存的。淘宝上随便找了家便宜的，号称全兼容。现在 DDR4 应该都很成熟了，最后也是不出意外一次点亮。

![](https://image.mitsea.com/blog/posts/2023/03/%E7%BE%A4%E6%99%96%20DS1621%2B%20%E5%85%A5%E6%89%8B%E4%B8%8E%E9%85%8D%E7%BD%AE/IMG_0447.avif)

### 网卡

这台机器的 PCIe 插槽虽然是 x8，但实际上只有 x4 的速度。所以如果想用双网口最好是选择 PCIe 3.0 的网卡，X520 就被排除了，于是选择了市面上目前最便宜的浪潮拆机 X710。群晖自带驱动，直接就能用。

![](https://image.mitsea.com/blog/posts/2023/03/%E7%BE%A4%E6%99%96%20DS1621%2B%20%E5%85%A5%E6%89%8B%E4%B8%8E%E9%85%8D%E7%BD%AE/IMG_0491.avif)

### 机械硬盘

由于我的数据包含异地备份一共有四份，所以我完全不担心硬盘导致的数据损坏。选择了四块拆机的东芝 12T 老硬盘，一块 660，价格还成。这个容量足够我使用，所以没有买更大的 16T 或者 18T。

![](https://image.mitsea.com/blog/posts/2023/03/%E7%BE%A4%E6%99%96%20DS1621%2B%20%E5%85%A5%E6%89%8B%E4%B8%8E%E9%85%8D%E7%BD%AE/IMG_0980.avif)

### SATA SSD

最近比较火的大容量 SATA SSD，eMLC 颗粒，持续写入速度和满盘时的读写速度都不错，很适合拿来做缓存盘。

这里提醒一下，群晖的缓存盘用到后面基本都是满盘读写，所以一定要选择带外置缓存的 SSD，否则在读取数据的时候，由于无缓SSD满盘缓外随机写入弱鸡，反而会卡 I/O。

![](https://image.mitsea.com/blog/posts/2023/03/%E7%BE%A4%E6%99%96%20DS1621%2B%20%E5%85%A5%E6%89%8B%E4%B8%8E%E9%85%8D%E7%BD%AE/IMG_1073.avif)

### nvme SSD

闲置的两个 SN730，原来放在游戏电脑里存游戏的。现在 SSD 便宜了，电脑都换上了 2T 以上的大硬盘，1T 就被淘汰下来了。

![](https://image.mitsea.com/blog/posts/2023/03/%E7%BE%A4%E6%99%96%20DS1621%2B%20%E5%85%A5%E6%89%8B%E4%B8%8E%E9%85%8D%E7%BD%AE/IMG_0149.avif)

Photo by [Tom Mrazek](https://unsplash.com/@tommrazek?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/nas?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
  