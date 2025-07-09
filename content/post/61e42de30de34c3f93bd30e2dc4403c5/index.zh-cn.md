+++
author = "FlintyLemming"
title = "QNAP 设置桥接"
slug = "61e42de30de34c3f93bd30e2dc4403c5"
date = "2021-04-02"
description = ""
categories = ["HomeLab", "Network"]
tags = ["QNAP", "NAS"]
image = "https://img.mitsea.com/blog/posts/2021/04/QNAP%20%E8%AE%BE%E7%BD%AE%E6%A1%A5%E6%8E%A5/title.avif"
+++

最近购入了一台 QNAP TS-532X NAS，这是一台万兆交换机。想要两个设备之间以万兆的速度连接，除了两个设备都要有万兆网卡，往往会认为他们必须要接到一个万兆交换机上才能用，就像这样。

![](https://img.mitsea.com/blog/posts/2021/04/QNAP%20%E8%AE%BE%E7%BD%AE%E6%A1%A5%E6%8E%A5/1.avif)

但其实不必，通过桥接 NAS 的 1GbE 和 10GbE 两个网口，就可以让 NAS 直接以 10Gbps 承载另一个有万兆网卡的设备，就像这样。

![](https://img.mitsea.com/blog/posts/2021/04/QNAP%20%E8%AE%BE%E7%BD%AE%E6%A1%A5%E6%8E%A5/2.avif)

按照我的理解（可能不正确），TS-532X 首先自己内部生成了一个虚拟交换机，交换机的上联口是到路由器，绑定 1GbE 网口。下联口一个是内部虚拟的，通给 NAS 本身，另一个绑定 10GbE 网口，通给另一台万兆设备。

下面介绍下如何设置：

## NAS 设置

1. 打开 网络与虚拟交换机，左下角，切换到 高级，点击 新增

    ![](https://img.mitsea.com/blog/posts/2021/04/QNAP%20%E8%AE%BE%E7%BD%AE%E6%A1%A5%E6%8E%A5/3.avif)

2. 点击 基本模式

    ![](https://img.mitsea.com/blog/posts/2021/04/QNAP%20%E8%AE%BE%E7%BD%AE%E6%A1%A5%E6%8E%A5/4.avif)

3. 分别勾选 1GbE 和 10GbE 两个网口，勾选下方的”启用扩展树协议“，点 应用

    ![](https://img.mitsea.com/blog/posts/2021/04/QNAP%20%E8%AE%BE%E7%BD%AE%E6%A1%A5%E6%8E%A5/5.avif)

4. 回到 总览，就可以看到桥接成功了。不过这里的拓补图讲道理有点迷惑，比如他这个交换机也有 IP（按理说他这个应该算是无网管交换机，应该没有 IP），然后 10GbE 的那个口 IP 跟 NAS 是一样的，总之有点迷惑，不用管它。

    ![](https://img.mitsea.com/blog/posts/2021/04/QNAP%20%E8%AE%BE%E7%BD%AE%E6%A1%A5%E6%8E%A5/6.avif)

## PC 设置

按理说此时直接用 10G 网线连接 PC 和 NAS 就可以直接用了，但是 QNAP 这个我不知道为什么，如果你不对这个虚拟交换机设置 DHCP 服务器，PC 就没法自动获取 IP 并联网。我担心设置 DHCP 服务器后跟路由器里的 DHCP 服务冲突，就没有开了。但这样的话，需要给 PC 手动指定一个 IP 地址。

1. 打开 控制面板 - 网络和 Internet - 网络和共享中心 - 更改适配器设置，可以看到 10GbE 的口已经启用了。当然一开始这边可能显示无网络权限啥的，我这边是已经配置好了的。

    ![](https://img.mitsea.com/blog/posts/2021/04/QNAP%20%E8%AE%BE%E7%BD%AE%E6%A1%A5%E6%8E%A5/7.avif)

2. 双击打开这个网络连接，然后点击 属性 - Internet 协议版本 4

    ![](https://img.mitsea.com/blog/posts/2021/04/QNAP%20%E8%AE%BE%E7%BD%AE%E6%A1%A5%E6%8E%A5/8.avif)

3. IP 地址设置一个跟内网其他设备不冲突的 IP 地址，子网掩码、网关、默认DNS地址和内网中其他设备一致

    ![](https://img.mitsea.com/blog/posts/2021/04/QNAP%20%E8%AE%BE%E7%BD%AE%E6%A1%A5%E6%8E%A5/9.avif)

4. 设置完毕

    ![](https://img.mitsea.com/blog/posts/2021/04/QNAP%20%E8%AE%BE%E7%BD%AE%E6%A1%A5%E6%8E%A5/10.avif)