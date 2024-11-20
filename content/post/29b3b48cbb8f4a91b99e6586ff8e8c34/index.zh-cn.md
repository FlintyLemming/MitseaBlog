+++
author = "FlintyLemming"
title = "修改 Windows 网络连接位置"
slug = "29b3b48cbb8f4a91b99e6586ff8e8c34"
date = "2020-02-26"
description = "Windows 的防火墙设置"
categories = ["Windows"]
tags = ["防火墙"]
image = "https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2020/02/%E4%BF%AE%E6%94%B9%20Windows%20%E7%BD%91%E7%BB%9C%E8%BF%9E%E6%8E%A5%E4%BD%8D%E7%BD%AE/title.avif"
+++

Windows Defender 防火墙中，网络连接的安全划分成 专用网络 和 来宾或公用网络

![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2020/02/%E4%BF%AE%E6%94%B9%20Windows%20%E7%BD%91%E7%BB%9C%E8%BF%9E%E6%8E%A5%E4%BD%8D%E7%BD%AE/1.avif)

如果使用后者，在系统的一些设置上可能会有安全性问题，下面介绍如何修改网络连接位置。其实方法有很多，但是 Windows 设置一直在变，这里介绍一个通用的方法。

1. Win + R 打开 运行。运行 secpol.msc，打开 本地安全策略

    ![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2020/02/%E4%BF%AE%E6%94%B9%20Windows%20%E7%BD%91%E7%BB%9C%E8%BF%9E%E6%8E%A5%E4%BD%8D%E7%BD%AE/2.avif)

2. 左边找到 网络列表管理器策略，右边双击有名称的网络，打开属性

    ![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2020/02/%E4%BF%AE%E6%94%B9%20Windows%20%E7%BD%91%E7%BB%9C%E8%BF%9E%E6%8E%A5%E4%BD%8D%E7%BD%AE/3.avif)

3. 在 网络位置 选项卡里，将 位置类型 改成专用

    ![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2020/02/%E4%BF%AE%E6%94%B9%20Windows%20%E7%BD%91%E7%BB%9C%E8%BF%9E%E6%8E%A5%E4%BD%8D%E7%BD%AE/4.avif)

4. 同理，将 无法识别的网络 的 位置类型 也改为 专用
5. 回到控制面板的防火墙那里查看状态。或者在 PowerShell 里执行下面的命令查看。

        Get-NetConnectionProfile

    ![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2020/02/%E4%BF%AE%E6%94%B9%20Windows%20%E7%BD%91%E7%BB%9C%E8%BF%9E%E6%8E%A5%E4%BD%8D%E7%BD%AE/5.avif)