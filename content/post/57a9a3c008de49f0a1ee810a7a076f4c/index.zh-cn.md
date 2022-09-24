+++
author = "FlintyLemming"
title = "M1 Mac 无法设置正确的缩放比例"
slug = "57a9a3c008de49f0a1ee810a7a076f4c"
date = "2021-03-03"
description = ""
categories = ["Apple"]
tags = ["M1", "Mac"]
image = "https://img.mitsea.com/blog/posts/2021/03/M1%20Mac%20%E6%97%A0%E6%B3%95%E8%AE%BE%E7%BD%AE%E6%AD%A3%E7%A1%AE%E7%9A%84%E7%BC%A9%E6%94%BE%E6%AF%94%E4%BE%8B/title.jpg?x-oss-process=style/ImageCompress"
+++

## 问题

关于 M1 Mac 的视频输出，我遇到的两个问题

1. **对于杂牌、冷门的 4K 显示器，M1 Mac 无法获取到正确的显示器分辨率。**
    
    比如我手头的松人 4K 显示器，就被识别成了一个分辨率为  1680x1050 的显示器
    
    ![](https://img.mitsea.com/blog/posts/2021/03/M1%20Mac%20%E6%97%A0%E6%B3%95%E8%AE%BE%E7%BD%AE%E6%AD%A3%E7%A1%AE%E7%9A%84%E7%BC%A9%E6%94%BE%E6%AF%94%E4%BE%8B/1.png?x-oss-process=style/ImageCompress)
    
2. **对于 4K 以下的分辨率，无法开启 HiDPI。**
    
    ![](https://img.mitsea.com/blog/posts/2021/03/M1%20Mac%20%E6%97%A0%E6%B3%95%E8%AE%BE%E7%BD%AE%E6%AD%A3%E7%A1%AE%E7%9A%84%E7%BC%A9%E6%94%BE%E6%AF%94%E4%BE%8B/2.png?x-oss-process=style/ImageCompress)
    
    同样的显示器，x86 Mac 没有问题
    
    ![](https://img.mitsea.com/blog/posts/2021/03/M1%20Mac%20%E6%97%A0%E6%B3%95%E8%AE%BE%E7%BD%AE%E6%AD%A3%E7%A1%AE%E7%9A%84%E7%BC%A9%E6%94%BE%E6%AF%94%E4%BE%8B/3.png?x-oss-process=style/ImageCompress)
    
    在 x86 Mac 上，2K 显示器如果系统没有开启 HiDPI，可以有很多方法开启，比如使用下面的这个脚本
    
    [xzhih/one-key-hidpi](https://github.com/xzhih/one-key-hidpi)
    
    但是 M1 Mac 目前没有任何方法强制开启 HiDPI。开发者解释原因如下：
    
    > 从 ioreg 的输出可以发现有差别，没有直接获取到完整的 EDID，但还是能获取到经过解析的部分 EDID 信息，与此脚本相关度最高的ProductID和LegacyManufacturerID(VendorID)，理论上单纯的开启 HIDPI 没啥问题，但脚本中存在的一些关于 EDID 的补丁(force RGB，fix 6bit color)，以及 EDID 的注入，实现起来比较麻烦（主要是通过已知信息反推补全、分辨率信息的补全）。
    > 
    
    > PS：在最新的M1芯片上，苹果的图像输出使用的是移动端(iPhone、iPad)用的 IOMobileFramebuffer(IOMFB)，而之前的设备上用的是 AppleIntelFramebuffer、AMDFramebuffer 等，这导致了以上的问题。
    > 
    
    [https://github.com/xzhih/one-key-hidpi/issues/157](https://github.com/xzhih/one-key-hidpi/issues/157)
    

## 解决方法？

我从淘宝购入了一个 edid 欺骗器，并让卖家烧写成主流的 Dell 4K 显示器信息，希望能有用。到时会更新。

![](https://img.mitsea.com/blog/posts/2021/03/M1%20Mac%20%E6%97%A0%E6%B3%95%E8%AE%BE%E7%BD%AE%E6%AD%A3%E7%A1%AE%E7%9A%84%E7%BC%A9%E6%94%BE%E6%AF%94%E4%BE%8B/4.png?x-oss-process=style/ImageCompress)

[https://item.taobao.com/item.htm?id=611036111443&tracelogww=ltckbburl](https://item.taobao.com/item.htm?id=611036111443&tracelogww=ltckbburl)

## 更新

淦，翻车了…不行

![](https://img.mitsea.com/blog/posts/2021/03/M1%20Mac%20%E6%97%A0%E6%B3%95%E8%AE%BE%E7%BD%AE%E6%AD%A3%E7%A1%AE%E7%9A%84%E7%BC%A9%E6%94%BE%E6%AF%94%E4%BE%8B/5.jpg?x-oss-process=style/ImageCompress)

> Photo by [Renato Ramos Puma](https://unsplash.com/@renatoramospuma?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
  