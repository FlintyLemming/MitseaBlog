+++
author = "FlintyLemming"
title = "QNAP TS-453B mini 黑群晖散热简单改造"
slug = "8d55622db32146919b583c6fd4e3b670"
date = "2025-01-28T01:00:00+08:00"
description = ""
categories = ["Consumer"]
tags = ["QNAP"]
image = "https://img.flinty.moe/blog/posts/2025/01/QNAP%20TS-453B%20mini%20%E9%BB%91%E7%BE%A4%E6%99%96%E6%95%A3%E7%83%AD%E7%AE%80%E5%8D%95%E6%94%B9%E9%80%A0/pawel-czerwinski-Zr7SggNN0ZI-unsplash.avif"
+++

讲道理买这台的原因是之前买了一台 TS-464C 来黑群晖，体验挺不错的，安装很顺利，散热也没问题，风扇可以自动调速，在成品 NAS 里体积可以说是最小的。所以我另一个地方需要一台性能不高的黑群晖我就首先考虑 QNAP 的前代主流产品——TS-453B mini。

但是这玩意真的不适合黑群晖，除了我在上一篇中提到的硬盘识别问题，散热也有问题。这玩意本身散热就很拉，然后黑群晖下风扇转速也很低，不能自动调速，所以硬盘温度爆炸。

![](https://img.flinty.moe/blog/posts/2025/01/QNAP%20TS-453B%20mini%20%E9%BB%91%E7%BE%A4%E6%99%96%E6%95%A3%E7%83%AD%E7%AE%80%E5%8D%95%E6%94%B9%E9%80%A0/%E5%9B%BE%E7%89%87_XiG9EvdWFA.avif)

下面简单记录一下改造过程，需要一个 PWM 风扇调速器和风扇 4pin 的外置供电。

外壳拆开后可以看到，这个机器完全就是靠后面的一个涡轮风扇抽风，然后向下吹风，所以这台机器一定要留出下出风空间

![](https://img.flinty.moe/blog/posts/2025/01/QNAP%20TS-453B%20mini%20%E9%BB%91%E7%BE%A4%E6%99%96%E6%95%A3%E7%83%AD%E7%AE%80%E5%8D%95%E6%94%B9%E9%80%A0/20250127_233928_j4s9uFsPd2.avif)

风扇 4PIN 原来是插在这里，风扇是标准的 4PIN PWM 风扇

![](https://img.flinty.moe/blog/posts/2025/01/QNAP%20TS-453B%20mini%20%E9%BB%91%E7%BE%A4%E6%99%96%E6%95%A3%E7%83%AD%E7%AE%80%E5%8D%95%E6%94%B9%E9%80%A0/20250127_233911_Dor-jX0sG6.avif)

把它的线缆引到下面内存插槽处

![](https://img.flinty.moe/blog/posts/2025/01/QNAP%20TS-453B%20mini%20%E9%BB%91%E7%BE%A4%E6%99%96%E6%95%A3%E7%83%AD%E7%AE%80%E5%8D%95%E6%94%B9%E9%80%A0/20250127_233959_CFYCUwd7PX.avif)

底盖是能正常盖上的，内存插槽盖板就不盖了，接外置电源

![](https://img.flinty.moe/blog/posts/2025/01/QNAP%20TS-453B%20mini%20%E9%BB%91%E7%BE%A4%E6%99%96%E6%95%A3%E7%83%AD%E7%AE%80%E5%8D%95%E6%94%B9%E9%80%A0/20250127_234037_aBfBtOp5r2.avif)

这样就可以随意调速了，我是有个单独的设备间所以不怕吵，我直接拉满。然后脚垫位置可以调一下，这样设备可以把前部悬空，留出更多的出风空间。

![](https://img.flinty.moe/blog/posts/2025/01/QNAP%20TS-453B%20mini%20%E9%BB%91%E7%BE%A4%E6%99%96%E6%95%A3%E7%83%AD%E7%AE%80%E5%8D%95%E6%94%B9%E9%80%A0/20250128_000400_LIc52SYE7i.avif)

调速器就藏在最下面

![](https://img.flinty.moe/blog/posts/2025/01/QNAP%20TS-453B%20mini%20%E9%BB%91%E7%BE%A4%E6%99%96%E6%95%A3%E7%83%AD%E7%AE%80%E5%8D%95%E6%94%B9%E9%80%A0/20250128_000415_vTtpgyA4yC.avif)

瞬间凉快 20 摄氏度

![](https://img.flinty.moe/blog/posts/2025/01/QNAP%20TS-453B%20mini%20%E9%BB%91%E7%BE%A4%E6%99%96%E6%95%A3%E7%83%AD%E7%AE%80%E5%8D%95%E6%94%B9%E9%80%A0/{C0F79B23-9A8D-4780-810D-FE56C981E7D1}_wQjhdzknNm.avif)

> Photo by [Pawel Czerwinski](https://unsplash.com/@pawel_czerwinski?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/a-blurry-photo-of-a-white-cloth-Zr7SggNN0ZI?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)
      