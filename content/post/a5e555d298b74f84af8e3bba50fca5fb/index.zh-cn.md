+++
author = "FlintyLemming"
title = "华为 AX3 Pro 与 小米 AX3600 的对比"
slug = "a5e555d298b74f84af8e3bba50fca5fb"
date = "2020-05-17"
description = "可惜华为这个芯片绝版了"
categories = ["Consumer"]
tags = ["路由器", "小米", "华为"]
image = "https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2020/05/%E5%8D%8E%E4%B8%BA%20AX3%20Pro%20%E4%B8%8E%20%E5%B0%8F%E7%B1%B3%20AX3600%20%E7%9A%84%E5%AF%B9%E6%AF%94/title.avif"
+++

家里是千兆宽带，并且新买的 iPhone SE 也支持 Wi-Fi 6，所以就买了个 Wi-Fi 6 路由器，在被华硕 AX3000 坑过一次后，我将目光瞄向了更加实惠的华为和小米。大家都说互联网公司出的路由器垃圾，但考虑到我之前一直用的华为 WS5200 四核版一直都没啥问题，所以这次，两家都买了，测试一下。

### 测试说明

环境较为复杂，有8个小米智能设备，一屋子大概总计 22 设备（开了不少虚拟机，桥接的）。然后开了 H@H，一直保持有外网大于 10 的 connection。pt 保种开着，同样有很多线程，但上传占用不大。信号衰减测不了，刚毕业，就一个人在外面，一间屋子。

宽带的话，月费 399。299 的千兆宽带，150G的5G流量。多的 100 是加的上传，上传现在是 200-400M之间，不是太稳定，看脸。 下面测试中，只要是大于 200Mbps 就是合格的，不需要过分纠结。

160MHz 的话，这次没有测试，因为小米的我这边没有收到推送。华为的 160MHz 需要华为自己的设备。而且 160MHz 对于环境要求非常苛刻，实用意义目前并不是太大。

价格的话，华为的在京东 289 元参加预约购得；小米在实体店 599 元购得。

废话不多说了，直接上测速图吧

### 有线基准

下载 935Mbps，上传 226Mbps

![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2020/05/%E5%8D%8E%E4%B8%BA%20AX3%20Pro%20%E4%B8%8E%20%E5%B0%8F%E7%B1%B3%20AX3600%20%E7%9A%84%E5%AF%B9%E6%AF%94/1.avif)

### Wi-Fi 6 80MHz

设备是 iPhone SE，左边是华为 AX3 Pro，右边是小米 AX 3600。小米稍快。

![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2020/05/%E5%8D%8E%E4%B8%BA%20AX3%20Pro%20%E4%B8%8E%20%E5%B0%8F%E7%B1%B3%20AX3600%20%E7%9A%84%E5%AF%B9%E6%AF%94/2.avif)

### Wi-Fi 5 866Mbps

设备是 iPad mini 5，左边是华为 AX3 Pro，右边是小米 AX 3600。小米稍快。

![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2020/05/%E5%8D%8E%E4%B8%BA%20AX3%20Pro%20%E4%B8%8E%20%E5%B0%8F%E7%B1%B3%20AX3600%20%E7%9A%84%E5%AF%B9%E6%AF%94/3.avif)

### Wi-Fi 5 433Mbps

设备是小米 MIX 1，左边是华为 AX3 Pro，右边是小米 AX 3600。华为稍快。

![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2020/05/%E5%8D%8E%E4%B8%BA%20AX3%20Pro%20%E4%B8%8E%20%E5%B0%8F%E7%B1%B3%20AX3600%20%E7%9A%84%E5%AF%B9%E6%AF%94/4.avif)

### Wi-Fi 6 内网传输

由于 iperf 测试结果个人认为脱离实际，所以方法是使用网络 sftp 往 iPhone 里传输文件，图中注意 Edge 的上传速度即可，左边是华为 AX3 Pro，右边是小米 AX 3600。小米稍快。

![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2020/05/%E5%8D%8E%E4%B8%BA%20AX3%20Pro%20%E4%B8%8E%20%E5%B0%8F%E7%B1%B3%20AX3600%20%E7%9A%84%E5%AF%B9%E6%AF%94/5.avif)

## 总结

这两款的路由器速度的表现都大大超过了之前买的华硕 AX3000，我是比较满意的。多项对比下来，华为表现稍差一些，不过和小米差距并不大。况且华为也便宜了不少，造型也更加美观。至于买哪一个，我觉得凭自己对品牌的喜好即可，不需要强求太多。