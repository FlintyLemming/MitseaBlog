+++
author = "FlintyLemming"
title = "简要解释 SRC-IP"
slug = "4b935dae96ff43d595486a8cce2d42e1"
date = "2019-10-27"
description = ""
categories = ["Network"]
tags = ["Surge"]
image = ""
+++

在新版本的 Surge 中增加了个 SRC-IP 类型的分流规则，从来没听说过这个，引起了极大的兴趣。简单看了下大致理解了 SRC-IP。

简要来说，哪个程序或设备带来了流量，这个程序或设备的 IP 地址，就是这些流量的 SRC-IP。

下面，就以 Switch 内网代理为例子简单解释。至于和负载均衡搭配使用的公网用法，以后再说。

Switch 虽然可以直连，但商店访问慢，下载慢，最关键的是有的游戏直连甚至无法游玩，比如 Asphalt 9

![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2019/10/%E7%AE%80%E8%A6%81%E8%A7%A3%E9%87%8A%20SRC-IP/1.avif)

一般来说有三种方式，加速器、路由器弄代理，以及今天用到的 HTTP 代理。哪个方式好这里不做讨论，各有优势。

首先保证 Surge 已经开启了 HTTP 代理服务

![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2019/10/%E7%AE%80%E8%A6%81%E8%A7%A3%E9%87%8A%20SRC-IP/2.avif)

Switch 上，设置 - 互联网 - 互联网设置 - 点击已经连接的 Wi-Fi - 更改设置 - 代理服务器设置 这里点开，启用它。服务器地址一定不要填写 0.0.0.0，而是你运行 Surge 的设备的内网地址，macOS 版的可以在这里看到

![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2019/10/%E7%AE%80%E8%A6%81%E8%A7%A3%E9%87%8A%20SRC-IP/3.avif)

端口填写的是 HTTP 代理的端口，不要填 SOCKS5 的，自动验证保持为不启用。保存后重新连接。

![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2019/10/%E7%AE%80%E8%A6%81%E8%A7%A3%E9%87%8A%20SRC-IP/4.avif)

此时 Switch 不一定能正常加速，这是因为，Switch 的流量发送到 Surge 后，要进行规则判断，如果你的规则里没有 Switch 网络活动相关的规则，则还是直连。当然，你也可以通过抓包看域名手动添加对应的规则。但考虑到 Switch 上的服务即使直连都比较慢，不如全部都走代理，那么怎么实现呢？这就引出了 SRC-IP 的概念。在 Surge 的 Dashboard 里，你可以看到很多 Switch 的流量，但是这些流量都是又哪一个 IP 地址带来的呢？就是 Switch 机器的内网 ip

![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2019/10/%E7%AE%80%E8%A6%81%E8%A7%A3%E9%87%8A%20SRC-IP/5.avif)

我这里就是 192.168.3.56，这就是这些流量的 SRC-IP。只要对这一个 SRC-IP 进行代理，他发来的所有流量就全部走代理了。你可以右键该设备，直接添加一个 SRC-IP 的规则。

![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2019/10/%E7%AE%80%E8%A6%81%E8%A7%A3%E9%87%8A%20SRC-IP/6.avif)

当然，添加之前，可以创建一个专门给 Switch 用的策略组，方便出现问题后可以切回 Direct

![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2019/10/%E7%AE%80%E8%A6%81%E8%A7%A3%E9%87%8A%20SRC-IP/7.avif)

再次检查 Dashboard 里 Switch 的流量，即可发现游戏和服务的流量，根据 SRC-IP 规则的判断，走了代理。

![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2019/10/%E7%AE%80%E8%A6%81%E8%A7%A3%E9%87%8A%20SRC-IP/8.avif)

游戏自然也是能够正常进入

![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2019/10/%E7%AE%80%E8%A6%81%E8%A7%A3%E9%87%8A%20SRC-IP/9.avif)