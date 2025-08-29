+++
author = "FlintyLemming"
title = "在群晖上配置 iSCSI 并在 Windows 下使用"
slug = "00187875246c440fb8f50bb1354ce81b"
date = "2020-05-31"
description = ""
categories = ["HomeLab", "Windows"]
tags = ["Synology", "NAS"]
image = "https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/05/%E5%9C%A8%E7%BE%A4%E6%99%96%E4%B8%8A%E9%85%8D%E7%BD%AE%20iSCSI%20%E5%B9%B6%E5%9C%A8%20Windows%20%E4%B8%8B%E4%BD%BF%E7%94%A8/title.avif"
+++

最近电脑的硬盘不够啦，但我有个 NAS，里边好几块硬盘，存储是没问题，但是如果想跟电脑内置的硬盘一样用就有困难了。无论是直接安装在网络上的硬盘里的程序还是游戏，经常会有问题，于是经人提醒有一个 iSCSI 的功能。

于是就拿今天刚到的 2T 西数蓝盘试试水。关于机械硬盘这个问题，我觉得一般人来说买个西数蓝盘差不多了，同等级的希捷和东芝我都有接触过坏盘。说回 iSCSI，听说性能不是太好，不过我寻思着机械硬盘性能本来就那样，我就试试看呗，想看性能表现的直接看到最后就好。下面介绍配置步骤。

1. 在应用程序中找到 iSCSI Manager，打开

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/05/%E5%9C%A8%E7%BE%A4%E6%99%96%E4%B8%8A%E9%85%8D%E7%BD%AE%20iSCSI%20%E5%B9%B6%E5%9C%A8%20Windows%20%E4%B8%8B%E4%BD%BF%E7%94%A8/1.avif)

2. 点击左侧的 Target，点击后新增一个 iSCSI target，在第一步中什么都不用改，直接下一步

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/05/%E5%9C%A8%E7%BE%A4%E6%99%96%E4%B8%8A%E9%85%8D%E7%BD%AE%20iSCSI%20%E5%B9%B6%E5%9C%A8%20Windows%20%E4%B8%8B%E4%BD%BF%E7%94%A8/2.avif)

3. 这里也什么都不用改，直接新增一个 LUN

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/05/%E5%9C%A8%E7%BE%A4%E6%99%96%E4%B8%8A%E9%85%8D%E7%BD%AE%20iSCSI%20%E5%B9%B6%E5%9C%A8%20Windows%20%E4%B8%8B%E4%BD%BF%E7%94%A8/3.avif)

4. 这里先选择一个存储位置，再根据实际情况选择给这个服务分配多少空间，我这里就将这块 2T 的硬盘全部分配给这个服务

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/05/%E5%9C%A8%E7%BE%A4%E6%99%96%E4%B8%8A%E9%85%8D%E7%BD%AE%20iSCSI%20%E5%B9%B6%E5%9C%A8%20Windows%20%E4%B8%8B%E4%BD%BF%E7%94%A8/4.avif)

5. 检查一下，没什么问题后点应用就可以了。

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/05/%E5%9C%A8%E7%BE%A4%E6%99%96%E4%B8%8A%E9%85%8D%E7%BD%AE%20iSCSI%20%E5%B9%B6%E5%9C%A8%20Windows%20%E4%B8%8B%E4%BD%BF%E7%94%A8/5.avif)

6. 然后我们需要开启 Windows 里的 iSCSI 服务，直接使用搜索功能就可以搜索到配置工具

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/05/%E5%9C%A8%E7%BE%A4%E6%99%96%E4%B8%8A%E9%85%8D%E7%BD%AE%20iSCSI%20%E5%B9%B6%E5%9C%A8%20Windows%20%E4%B8%8B%E4%BD%BF%E7%94%A8/6.avif)

7. 第一次使用会让我们先把服务起起来，顺便会帮我们设置开机自启

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/05/%E5%9C%A8%E7%BE%A4%E6%99%96%E4%B8%8A%E9%85%8D%E7%BD%AE%20iSCSI%20%E5%B9%B6%E5%9C%A8%20Windows%20%E4%B8%8B%E4%BD%BF%E7%94%A8/7.avif)

8. 在“目标”里填上 NAS 的本地 IP 地址，然后点击“快速连接”

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/05/%E5%9C%A8%E7%BE%A4%E6%99%96%E4%B8%8A%E9%85%8D%E7%BD%AE%20iSCSI%20%E5%B9%B6%E5%9C%A8%20Windows%20%E4%B8%8B%E4%BD%BF%E7%94%A8/8.avif)

9. 这样就自动连上 NAS 的 iSCSI 服务了，点完成并确定上一级设置。

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/05/%E5%9C%A8%E7%BE%A4%E6%99%96%E4%B8%8A%E9%85%8D%E7%BD%AE%20iSCSI%20%E5%B9%B6%E5%9C%A8%20Windows%20%E4%B8%8B%E4%BD%BF%E7%94%A8/9.avif)

10. 然后我们需要挂载这个分区，通过搜索或者右键 Windows 菜单按钮，打开 磁盘管理 工具

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/05/%E5%9C%A8%E7%BE%A4%E6%99%96%E4%B8%8A%E9%85%8D%E7%BD%AE%20iSCSI%20%E5%B9%B6%E5%9C%A8%20Windows%20%E4%B8%8B%E4%BD%BF%E7%94%A8/10.avif)

11. 这里就直接出现了引导挂在新分区的窗口，保留为 GPT 格式的分区，不需要更改，直接点确定

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/05/%E5%9C%A8%E7%BE%A4%E6%99%96%E4%B8%8A%E9%85%8D%E7%BD%AE%20iSCSI%20%E5%B9%B6%E5%9C%A8%20Windows%20%E4%B8%8B%E4%BD%BF%E7%94%A8/11.avif)

12. 之后直接和计算机内的物理磁盘一样的操作，对着新挂载的硬盘直接新建简单卷

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/05/%E5%9C%A8%E7%BE%A4%E6%99%96%E4%B8%8A%E9%85%8D%E7%BD%AE%20iSCSI%20%E5%B9%B6%E5%9C%A8%20Windows%20%E4%B8%8B%E4%BD%BF%E7%94%A8/12.avif)

13. 具体过程不赘述了，自己根据情况分配空间，我这里就分一个区了

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/05/%E5%9C%A8%E7%BE%A4%E6%99%96%E4%B8%8A%E9%85%8D%E7%BD%AE%20iSCSI%20%E5%B9%B6%E5%9C%A8%20Windows%20%E4%B8%8B%E4%BD%BF%E7%94%A8/13.avif)

14. 之后在资源管理器中就可以看到新盘符了，跟在电脑里的物理磁盘一样

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/05/%E5%9C%A8%E7%BE%A4%E6%99%96%E4%B8%8A%E9%85%8D%E7%BD%AE%20iSCSI%20%E5%B9%B6%E5%9C%A8%20Windows%20%E4%B8%8B%E4%BD%BF%E7%94%A8/14.avif)

    下面是关心的性能表现，我安装了一个大型程序，模拟硬盘满负载时候的表现，好像还不错呢

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/05/%E5%9C%A8%E7%BE%A4%E6%99%96%E4%B8%8A%E9%85%8D%E7%BD%AE%20iSCSI%20%E5%B9%B6%E5%9C%A8%20Windows%20%E4%B8%8B%E4%BD%BF%E7%94%A8/15.avif)

> Photo by [Patrick Lindenberg](https://unsplash.com/@heapdump?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/disk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
  