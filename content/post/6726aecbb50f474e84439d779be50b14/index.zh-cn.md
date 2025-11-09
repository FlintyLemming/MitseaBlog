+++
author = "FlintyLemming"
title = "群晖的 CloudSync，我劝你别用"
slug = "6726aecbb50f474e84439d779be50b14"
date = "2024-03-20"
description = "群晖的 CloudSync 在同步文件时，会因为某些问题导致不同步部分文件，并且 没 有 任 何 提 示"
categories = ["Consumer", "HomeLab"]
tags = ["群晖", "bug"]
image = "https://assets.mitsea.cn/blog/posts/2024/03/6726aecbb50f474e84439d779be50b14/rick-rothenberg-HTCLvTGXpmM-unsplash.avif"
+++

## 省流

群晖的 CloudSync 在同步文件时，会因为某些问题导致不同步部分文件，并且 **没 有 任 何 提 示**。

## 具体现象

我经常在 A 地的电脑上下载东西，然后再同步到 B 地。于是我选择使用群晖的 CloudSync 同步。具体就是我在 A 上面搭建一个 WebDAV（通过 Alist 实现），然后 B 地的群晖上使用 CloudSync 使用双向同步下载回来。

今天我瞟了眼两边的文件数，坏了，不对，漏同步文件了

![](https://assets.mitsea.cn/blog/posts/2024/03/6726aecbb50f474e84439d779be50b14/PixPin_2024-03-20_20-09-12.avif)

于是我就拉文件列表到 Excel 中对比，发现漏同步的文件含有在 Windows 上显示异常的特殊字符

![](https://assets.mitsea.cn/blog/posts/2024/03/6726aecbb50f474e84439d779be50b14/Untitled.avif)

![](https://assets.mitsea.cn/blog/posts/2024/03/6726aecbb50f474e84439d779be50b14/Untitled%201.avif)

其实这也不算是啥特殊字符，比如第一个是日语里的浊点（U+3099），在 Windows 上显示也是对的

![](https://assets.mitsea.cn/blog/posts/2024/03/6726aecbb50f474e84439d779be50b14/Untitled%202.avif)

第二个是韩语，在 Windows 上也是分开显示，但是在 Web 上显示是对的

![](https://assets.mitsea.cn/blog/posts/2024/03/6726aecbb50f474e84439d779be50b14/Untitled%203.avif)

之所以会分开，是因为编辑时使用的输入方式问题。以 ド 为例，如果是输入法候选里选择的 ド，那就会直接输入“ド” (U+30C9)，这是一个字符。但如果是靠系统渲染去拼字，输入时输入两个字符，即 “ト” (U+30C8) + 浊点（U+3099），则会出现上述情况。

显然，群晖的 CloudSync 至少在我这里没有处理好这个问题。更严重的是，它完全没有任何错误的提示，而是直接忽略了含有 组合用发音符号（combining diacritical mark）的文件。

如果它还有别的可能会导致直接不同步的情况，并且已经同步了大量文件的话，我想正常人根本不会发现自己信赖的异地备份其实根本就是不完整的备份。这对于一个专注数据管理与备份的系统来说我觉得是不应该的。

坑人……

> [Unsplash](https://unsplash.com/ja/%E5%86%99%E7%9C%9F/%E6%B3%A2%E7%B7%9A%E3%81%AE%E3%81%82%E3%82%8B%E9%9D%92%E3%81%A8%E7%B7%91%E3%81%AE%E8%83%8C%E6%99%AF-HTCLvTGXpmM?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)の[Rick Rothenberg](https://unsplash.com/ja/@rick_rothenberg?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)が撮影した写真
  