+++
author = "FlintyLemming"
title = "C盘剩余空间大于2T无法安装阿里云盘"
slug = "81dfe14f501b4eaeaaf121d3e68c3ad3"
date = "2023-09-05"
description = ""
categories = ["Windows", "LifeTec"]
tags = ["阿里云盘"]
image = "https://hf-public-source.mitsea.com:8840/images/blog/posts/2023/09/C%E7%9B%98%E5%89%A9%E4%BD%99%E7%A9%BA%E9%97%B4%E5%A4%A7%E4%BA%8E2T%E6%97%A0%E6%B3%95%E5%AE%89%E8%A3%85%E9%98%BF%E9%87%8C%E4%BA%91%E7%9B%98/boliviainteligente-5mwuSrKEN2U-unsplash.avif"
+++

在尝试安装阿里云盘的时候提示如下错误

![](https://hf-public-source.mitsea.com:8840/images/blog/posts/2023/09/C%E7%9B%98%E5%89%A9%E4%BD%99%E7%A9%BA%E9%97%B4%E5%A4%A7%E4%BA%8E2T%E6%97%A0%E6%B3%95%E5%AE%89%E8%A3%85%E9%98%BF%E9%87%8C%E4%BA%91%E7%9B%98/CleanShot%202023-09-05%20at%2011.32.19%402x.avif)

查看日志

![](https://hf-public-source.mitsea.com:8840/images/blog/posts/2023/09/C%E7%9B%98%E5%89%A9%E4%BD%99%E7%A9%BA%E9%97%B4%E5%A4%A7%E4%BA%8E2T%E6%97%A0%E6%B3%95%E5%AE%89%E8%A3%85%E9%98%BF%E9%87%8C%E4%BA%91%E7%9B%98/CleanShot%202023-09-05%20at%2011.36.41%402x.avif)

发现他认为 3506375848 小于 332140

这显然是发生了某种溢出，但是我不记得是什么类型的变量了，询问 ChatGPT 结果如下

在考虑数据溢出的情况下，332140 可能会被看作比 3524241260 大。例如，如果我们使用 32 位无符号整数来存储数据，那么数值的范围是 0 到 4294967295。但是，如果我们使用 32 位有符号整数，那么数值的范围是 -2147483648 到 2147483647。

假设我们使用 32 位有符号整数，3524241260 将超出正数范围并被解释为负数。具体地说，3524241260 的二进制表示超过了 32 位的最大正数表示，因此它的最高位（符号位）会被设置为 1，这意味着它是一个负数。

在这种情况下，332140 为正数，而 3524241260 被解释为负数，所以 332140 会大于 3524241260。

这种情况称为“整数溢出”。在进行数学计算或比较时，需要特别注意这种情况，以防止意外的结果。

2147483647 经过换算就是 2TB。由于临时目录默认就是C盘，所以只要C盘大于 2TB，就会导致无法安装阿里云盘。

解决方法可以是直接解压 .exe 安装文件，直接当成绿色软件用；或是想办法减少 C 盘可用空间；或是修改临时目录的环境变量路径到别的剩余空间较小的分区里。

> Photo by [BoliviaInteligente](https://unsplash.com/@boliviainteligente?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/photos/a-blurry-image-of-a-yellow-and-brown-background-5mwuSrKEN2U?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
  