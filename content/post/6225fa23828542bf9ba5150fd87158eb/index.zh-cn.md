+++
author = "FlintyLemming"
title = "Hyper-V 创建固定网关 IP 的内部网络"
slug = "6225fa23828542bf9ba5150fd87158eb"
date = "2023-10-21"
description = "HNS 全责"
categories = ["Windows", "Network"]
tags = ["Windows", "Hyper-V"]
image = "https://image.mitsea.com/blog/posts/2023/10/Hyper-V%20%E5%88%9B%E5%BB%BA%E5%9B%BA%E5%AE%9A%E7%BD%91%E5%85%B3%20IP%20%E7%9A%84%E5%86%85%E9%83%A8%E7%BD%91%E7%BB%9C/sander-weeteling-KABfjuSOx74-unsplash.avif"
+++

Hyper-V 默认会创建一个内部网络的交换机，而且连接到这个交换机的虚拟机可以正常访问互联网。但是这个交换机是由 HNS 创建的，HNS 网络服务主要是为容器服务的，内部有很多无法改动的预设配置。最头疼的就是他的网关地址在每次开机后都会发生变化。这篇文章介绍如何使用传统手动内部网络配置来创建一个网关 IP 不会变动的内部网络。

![](https://image.mitsea.com/blog/posts/2023/10/Hyper-V%20%E5%88%9B%E5%BB%BA%E5%9B%BA%E5%AE%9A%E7%BD%91%E5%85%B3%20IP%20%E7%9A%84%E5%86%85%E9%83%A8%E7%BD%91%E7%BB%9C/Untitled.avif)

1. 创建一个 内部网络 类型的虚拟交换机

    ![](https://image.mitsea.com/blog/posts/2023/10/Hyper-V%20%E5%88%9B%E5%BB%BA%E5%9B%BA%E5%AE%9A%E7%BD%91%E5%85%B3%20IP%20%E7%9A%84%E5%86%85%E9%83%A8%E7%BD%91%E7%BB%9C/Untitled%201.avif)

2. 找到有互联网的 Interface，双击它 - 属性 - 共享选项卡，这里选择刚才创建的虚拟交换机

    ![](https://image.mitsea.com/blog/posts/2023/10/Hyper-V%20%E5%88%9B%E5%BB%BA%E5%9B%BA%E5%AE%9A%E7%BD%91%E5%85%B3%20IP%20%E7%9A%84%E5%86%85%E9%83%A8%E7%BD%91%E7%BB%9C/Untitled%202.avif)

3. 这个虚拟交换机会自动设置一个网关 IP，网段默认为 192.168.137.0/24

    ![](https://image.mitsea.com/blog/posts/2023/10/Hyper-V%20%E5%88%9B%E5%BB%BA%E5%9B%BA%E5%AE%9A%E7%BD%91%E5%85%B3%20IP%20%E7%9A%84%E5%86%85%E9%83%A8%E7%BD%91%E7%BB%9C/Untitled%203.avif)

4. 虚拟机绑定这个虚拟交换机

    ![](https://image.mitsea.com/blog/posts/2023/10/Hyper-V%20%E5%88%9B%E5%BB%BA%E5%9B%BA%E5%AE%9A%E7%BD%91%E5%85%B3%20IP%20%E7%9A%84%E5%86%85%E9%83%A8%E7%BD%91%E7%BB%9C/Untitled%204.avif)

5. 默认直接就能拿到 IP，也可以自己手动修改一个静态地址。此时互联网访问也是正常的。

    ![](https://image.mitsea.com/blog/posts/2023/10/Hyper-V%20%E5%88%9B%E5%BB%BA%E5%9B%BA%E5%AE%9A%E7%BD%91%E5%85%B3%20IP%20%E7%9A%84%E5%86%85%E9%83%A8%E7%BD%91%E7%BB%9C/Untitled%205.avif)

> Photo by [Sander Weeteling](https://unsplash.com/@sanderweeteling?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/teal-bookeh-lights-KABfjuSOx74?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)
  