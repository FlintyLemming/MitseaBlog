+++
author = "FlintyLemming"
title = "关于 USB-C 实现4K 60Hz 和 USB 3.0 共存"
slug = "903cdebd35954bda81cfd9ab75ea2b00"
date = "2020-01-08"
description = ""
categories = ["LifeTec"]
tags = ["USB-C"]
image = "https://image.mitsea.com/blog/posts/2020/01/%E5%85%B3%E4%BA%8E%20USB-C%20%E5%AE%9E%E7%8E%B04K%2060Hz%20%E5%92%8C%20USB%203.0%20%E5%85%B1%E5%AD%98/miguel-hernandez-NKFPu0Iw8Ys-unsplash.avif"
+++

昨天看了 Mac云课堂 的新视频，虽然是个推广，但非常详细的介绍了为什么现在的 USB-C 扩展坞又能支持 4K 60Hz 和 USB 3.0 了。链接在下方

[https://www.youtube.com/watch?v=J2cCxH0_V90](https://www.youtube.com/watch?v=J2cCxH0_V90)

于是我又想起来很久以前群友的解释，还是非常到位的。原文我会放在后面，我这边简单解释一下：

就是原来 USB-C 视频输出的引脚由于使用了更新的协议，导致每个引脚的带宽变大了。所以有富足的引脚来同时实现 USB 3.0。

解答一下昨天很多群友在说 USB-C 不走 Thunderbolt 如何同时支持 4k / 60 和 USB 3 的问题…

USB-C alt mode 走 DisplayPort 有两种方式：① 四组高速差分都给 DisplayPort；② 两组高速差分给 DisplayPort，剩下给 USB 3。

对于 ①，可以跑全速的 DisplayPort，应该在任何情况下都可以跑 4k / 60 ( DisplayPort 1.2 )；如果你 USB-C 后面的显卡 ( DisplayPort 的源 ) 支持 DisplayPort 1.3 /1.4 且你的 USB-C mux 比较新支持 passthrough DisplayPort 1.3 / 1.4 且你的显示器支持 DisplayPort 1.3 / 1.4，那么可以直接输出 5k / 60。USB 只有 2.0。

对于 ②，DisplayPort 只有一半的速度，应该在任何情况下都可以跑 2k / 60 ( DisplayPort 1.2 )；如果你 USB-C 后面的显卡 ( DisplayPort 的源 ) 支持 DisplayPort 1.3 /1.4 且你的 USB-C mux 比较新支持 passthrough DisplayPort 1.3 / 1.4 且你的显示器支持 DisplayPort 1.3 / 1.4，那么可以直接输出 4k / 60。USB 可以有 SS5 或者 SS10 的速度 ( 也是看 USB-C 后端接的 XHCI 的速度，Thunderbolt 的主控一般支持 SS10，其他可能只支持 SS5 )。

这两种模式在 DisplayPort 的网站上有说明：https://www.displayport.org/displayport-over-usb-c/

转接出来的非 DisplayPort 的视频信号，我这可以说 99% 的情况都是先通过 USB-C Alt mode 出 DisplayPort 信号再转成对应的视频信号；转接出来的非 USB 的非视频的其他接口 ( RJ-45 / 读卡器之类的 ) 100% 是通过 USB 转的。

这里不讨论 DisplayLink 这种虚拟显示器 + 视频捕捉 + USB 传输的方案。

有任何错误，欢迎到我们的讨论群 ( @CE_Observe_Chat ) 提出。

--------

果子那个新的同时支持 4k / 60 的 HDMI + USB 3.0 的 dongle，应该是用 ② 的方式，拉一条半速 DisplayPort 1.3，再转成 HDMI 2.0，由于有效带宽只有 32.4 Gbps / 2 * 0.8 = 12.96 Gbps，所以只能跑 4k / 60，不能 HDR / 10 bit ( 强上的话就不是 RGB / 444 了 )。并且他需要特定机型支持 ( 新款的 MacBook Pro 15 inch，接的独显 + mux 支持 passthrough DisplayPort 1.3 )。

> Photo by [Miguel Hernández](https://unsplash.com/@miguelheezg?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
  