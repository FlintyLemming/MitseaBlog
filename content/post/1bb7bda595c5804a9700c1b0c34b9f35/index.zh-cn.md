+++
author = "FlintyLemming"
title = "南京广电宽带浅测"
slug = "1bb7bda595c5804a9700c1b0c34b9f35"
date = "2025-03-19"
description = "难道真的有既好又快又便宜的宽带？"
categories = ["Network"]
tags = ["广电宽带"]
image = "https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2025/03/%E5%8D%97%E4%BA%AC%E5%B9%BF%E7%94%B5%E5%AE%BD%E5%B8%A6%E6%B5%85%E6%B5%8B/pawel-czerwinski-L3G482epINk-unsplash.avif"
+++

## 概述

价格：300M 299元/2年；600M 399元/2年；1000M 720元/2年，不开手机卡，无额外收费，默认桥接，没有 IPv6，送一个普通路由器。我办的 600M 的。

国内广电、电信、移动、联通都有线路，整体上无异常，和对应线路的家宽质量一致。跨省下载正常，不需要开 bbr，上传到某些地区可能跑不满

国外走移动和联通隧道，整体表现良好

晚高峰出口线路不变，没有刻意 QoS，在白天超过 500Mbps 的线路可能因为出口带宽不够有些微降速

## 本地

600Mbps，没有余量

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2025/03/%E5%8D%97%E4%BA%AC%E5%B9%BF%E7%94%B5%E5%AE%BD%E5%B8%A6%E6%B5%85%E6%B5%8B/image.avif)

## 出口 IP

[https://ip.skk.moe/multi](https://ip.skk.moe/multi)

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2025/03/%E5%8D%97%E4%BA%AC%E5%B9%BF%E7%94%B5%E5%AE%BD%E5%B8%A6%E6%B5%85%E6%B5%8B/image%201.avif)

## CloudFlare

cf 的流控比较迷，看不懂，速度也不稳定，仅供参考

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2025/03/%E5%8D%97%E4%BA%AC%E5%B9%BF%E7%94%B5%E5%AE%BD%E5%B8%A6%E6%B5%85%E6%B5%8B/image%202.avif)

## 稳定性和丢包

国内移动线路不丢包

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2025/03/%E5%8D%97%E4%BA%AC%E5%B9%BF%E7%94%B5%E5%AE%BD%E5%B8%A6%E6%B5%85%E6%B5%8B/image%203.avif)

国外移动线路基本不丢

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2025/03/%E5%8D%97%E4%BA%AC%E5%B9%BF%E7%94%B5%E5%AE%BD%E5%B8%A6%E6%B5%85%E6%B5%8B/image%204.avif)

## 自建节点测试

### 加拿大家宽

联通出口 200Mbps 带宽（目标地址上行 300Mbps），晚高峰不变

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2025/03/%E5%8D%97%E4%BA%AC%E5%B9%BF%E7%94%B5%E5%AE%BD%E5%B8%A6%E6%B5%85%E6%B5%8B/image%205.avif)

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2025/03/%E5%8D%97%E4%BA%AC%E5%B9%BF%E7%94%B5%E5%AE%BD%E5%B8%A6%E6%B5%85%E6%B5%8B/image%206.avif)

### HKT 家宽

联通出口 500Mbps 带宽（目标地址上行 500Mbps+），晚高峰不变

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2025/03/%E5%8D%97%E4%BA%AC%E5%B9%BF%E7%94%B5%E5%AE%BD%E5%B8%A6%E6%B5%85%E6%B5%8B/image%207.avif)

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2025/03/%E5%8D%97%E4%BA%AC%E5%B9%BF%E7%94%B5%E5%AE%BD%E5%B8%A6%E6%B5%85%E6%B5%8B/image%208.avif)

### AWS Lightsail 新加坡

移动出口 600Mbps 带宽（目标地址上行 1Gbps+）

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2025/03/%E5%8D%97%E4%BA%AC%E5%B9%BF%E7%94%B5%E5%AE%BD%E5%B8%A6%E6%B5%85%E6%B5%8B/image%209.avif)

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2025/03/%E5%8D%97%E4%BA%AC%E5%B9%BF%E7%94%B5%E5%AE%BD%E5%B8%A6%E6%B5%85%E6%B5%8B/image%2010.avif)

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2025/03/%E5%8D%97%E4%BA%AC%E5%B9%BF%E7%94%B5%E5%AE%BD%E5%B8%A6%E6%B5%85%E6%B5%8B/image%2011.avif)

晚高峰有些许降速

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2025/03/%E5%8D%97%E4%BA%AC%E5%B9%BF%E7%94%B5%E5%AE%BD%E5%B8%A6%E6%B5%85%E6%B5%8B/image%2012.avif)

### 合肥移动专线

50Mbps 带宽（目标地址上下行 50Mbps），晚高峰不变

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2025/03/%E5%8D%97%E4%BA%AC%E5%B9%BF%E7%94%B5%E5%AE%BD%E5%B8%A6%E6%B5%85%E6%B5%8B/image%2013.avif)

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2025/03/%E5%8D%97%E4%BA%AC%E5%B9%BF%E7%94%B5%E5%AE%BD%E5%B8%A6%E6%B5%85%E6%B5%8B/image%2014.avif)

### 中山联通商务极光

350Mbps+ 带宽（目标地址上下行 300Mbps+）

上传异常，下载正常，开关 bbr 结果一致

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2025/03/%E5%8D%97%E4%BA%AC%E5%B9%BF%E7%94%B5%E5%AE%BD%E5%B8%A6%E6%B5%85%E6%B5%8B/image%2015.avif)

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2025/03/%E5%8D%97%E4%BA%AC%E5%B9%BF%E7%94%B5%E5%AE%BD%E5%B8%A6%E6%B5%85%E6%B5%8B/image%2016.avif)

晚高峰上行正常了一些

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2025/03/%E5%8D%97%E4%BA%AC%E5%B9%BF%E7%94%B5%E5%AE%BD%E5%B8%A6%E6%B5%85%E6%B5%8B/image%2017.avif)

> Photo by [Pawel Czerwinski](https://unsplash.com/@pawel_czerwinski?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/a-wall-that-has-a-bunch-of-white-squares-on-it-L3G482epINk?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)
      