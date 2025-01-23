+++
author = "FlintyLemming"
title = "群晖外接 u.2 nvme 可行性验证"
slug = "9e866c458c9b48819386331eb7a68c57"
date = "2023-04-25"
description = "全闪 NAS 的一小步"
categories = ["HomeLab"]
tags = ["全闪", "NAS", "SSD"]
image = "https://image.mitsea.com/blog/posts/2023/04/%E7%BE%A4%E6%99%96%E5%A4%96%E6%8E%A5%20u.2%20nvme%20%E5%8F%AF%E8%A1%8C%E6%80%A7%E9%AA%8C%E8%AF%81/jorgen-haland-fu0z-_iPa4M-unsplash.avif"
+++

最近 SSD 很便宜，但是普通消费级一般是 2T 居多，想看便宜大盘只能买 u.2 nvme 盘。最近全新铠侠 CD6 7.68T 只要 2000 左右，加上手里有一块去年买的 3.84T 的小海豚 PB5，打算把这俩盘直接装到群晖里做存储池。解决思路其实很简单，第一是要把群晖内部 m.2 引出来并转接成 u.2 接口；第二是要外接 u.2 的 12V 供电。我就把配件买一买，试试看能不能用。

我的机器是 DS1621+，m.2 在机器内部，反而没有四盘位的 DS420+ 这种方便。

## m.2 延长线

延长线我买的 ADTLink 的延长线，买了两种材质，都可以用

![](https://image.mitsea.com/blog/posts/2023/04/%E7%BE%A4%E6%99%96%E5%A4%96%E6%8E%A5%20u.2%20nvme%20%E5%8F%AF%E8%A1%8C%E6%80%A7%E9%AA%8C%E8%AF%81/IMG_1097.avif)

到手后按照下图插上后延长到机身外

![](https://image.mitsea.com/blog/posts/2023/04/%E7%BE%A4%E6%99%96%E5%A4%96%E6%8E%A5%20u.2%20nvme%20%E5%8F%AF%E8%A1%8C%E6%80%A7%E9%AA%8C%E8%AF%81/IMG_1047.avif)

![](https://image.mitsea.com/blog/posts/2023/04/%E7%BE%A4%E6%99%96%E5%A4%96%E6%8E%A5%20u.2%20nvme%20%E5%8F%AF%E8%A1%8C%E6%80%A7%E9%AA%8C%E8%AF%81/IMG_1048.avif)

![](https://image.mitsea.com/blog/posts/2023/04/%E7%BE%A4%E6%99%96%E5%A4%96%E6%8E%A5%20u.2%20nvme%20%E5%8F%AF%E8%A1%8C%E6%80%A7%E9%AA%8C%E8%AF%81/IMG_1049.avif)

## m.2 转 u.2 转接线

注意如果是 CD6 这种盘，最好是买这种先转 8639 再转 u.2 的线。安费诺原厂的线似乎不支持 4.0，卖家说是无法使用，我也不确定是会降速还是不能用。

![](https://image.mitsea.com/blog/posts/2023/04/%E7%BE%A4%E6%99%96%E5%A4%96%E6%8E%A5%20u.2%20nvme%20%E5%8F%AF%E8%A1%8C%E6%80%A7%E9%AA%8C%E8%AF%81/IMG_1098.avif)

## 12V 供电

12V 供电可以买普通 12V 圆口电源提供

![](https://image.mitsea.com/blog/posts/2023/04/%E7%BE%A4%E6%99%96%E5%A4%96%E6%8E%A5%20u.2%20nvme%20%E5%8F%AF%E8%A1%8C%E6%80%A7%E9%AA%8C%E8%AF%81/IMG_1102.avif)

但是这样会非常扭曲，因为我没找到 DC 转 SATA 供电的，只找到 SATA 转 DC 供电的。所以如果这样的话就是 电源转大4pin转SATA供电，不过我试过，能用的，没问题。

买 DC 转大 4pin 时注意它接的引脚，一定要接 12V 的。电源也要注意最大电流，CD6 最大电流是 2.6A，这导致 PASS 了很多直接提供大4pin供电的 DC 电源。

我最后采用的是 ATX 电源供电方案，因为我正好闲置一个电源。

配合开机线，最后就像这样子。（忽略左下的 eSATA 扩展柜）

![](https://image.mitsea.com/blog/posts/2023/04/%E7%BE%A4%E6%99%96%E5%A4%96%E6%8E%A5%20u.2%20nvme%20%E5%8F%AF%E8%A1%8C%E6%80%A7%E9%AA%8C%E8%AF%81/IMG_1004.avif)

## 成果

简单整理一下，再加一个风扇，最后效果就像这样。

![](https://image.mitsea.com/blog/posts/2023/04/%E7%BE%A4%E6%99%96%E5%A4%96%E6%8E%A5%20u.2%20nvme%20%E5%8F%AF%E8%A1%8C%E6%80%A7%E9%AA%8C%E8%AF%81/IMG_1050.avif)

![](https://image.mitsea.com/blog/posts/2023/04/%E7%BE%A4%E6%99%96%E5%A4%96%E6%8E%A5%20u.2%20nvme%20%E5%8F%AF%E8%A1%8C%E6%80%A7%E9%AA%8C%E8%AF%81/Untitled.avif)

至于 nvme 做存储池，网上教程一大把，按照步骤创建存储池即可。

> Photo by [Jørgen Håland](https://unsplash.com/@jhaland?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)