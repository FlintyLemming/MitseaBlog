+++
author = "FlintyLemming"
title = "群晖 btrfs 关于 cow 的观察"
slug = "164dd22cef594dbcbeccf8b839a455c6"
date = "2022-11-06"
description = ""
categories = ["HomeLab"]
tags = ["DSM", "pt"]
image = "https://img.mitsea.com/blog/posts/2022/11/%E7%BE%A4%E6%99%96%20btrfs%20%E5%85%B3%E4%BA%8E%20cow%20%E7%9A%84%E8%A7%82%E5%AF%9F/title.jpg?x-oss-process=style/ImageCompress"
+++

## 关于 btrfs 的 cow

群晖 DSM 默认使用 btrfs 文件系统，这个fs的一大特性就是支持 cow。cow 使得在复制一份文件时不会额外占用重复的空间。

在系统设置中，打开这个开关即可启用该功能

![](https://img.mitsea.com/blog/posts/2022/11/%E7%BE%A4%E6%99%96%20btrfs%20%E5%85%B3%E4%BA%8E%20cow%20%E7%9A%84%E8%A7%82%E5%AF%9F/1.png?x-oss-process=style/ImageCompress)

这个本质上是在复制文件的时候在 cp 命令后追加 `--reflink=always` 参数。此外，cow 支持 smb，通过 smb 进行复制的操作同样有效，与硬链接相比有无可比拟的便利性。

## 小实验

这里有一个文件夹，里面有一个大小为 11.55GB 的文件。此时存储空间用量为 7.3 TB

![](https://img.mitsea.com/blog/posts/2022/11/%E7%BE%A4%E6%99%96%20btrfs%20%E5%85%B3%E4%BA%8E%20cow%20%E7%9A%84%E8%A7%82%E5%AF%9F/2.png?x-oss-process=style/ImageCompress)

在启用快速复制后，再复制出来 10 份相同文件，仅仅是文件名不同

![](https://img.mitsea.com/blog/posts/2022/11/%E7%BE%A4%E6%99%96%20btrfs%20%E5%85%B3%E4%BA%8E%20cow%20%E7%9A%84%E8%A7%82%E5%AF%9F/3.png?x-oss-process=style/ImageCompress)

此时发现存储空间用量还是 7.3TB，说明多复制出来的 10 个文件没有额外占用空间。这个很好理解，毕竟虽然文件名不同，但是这些文件的 md5 值都是一样的，本质还是同一个文件。

![](https://img.mitsea.com/blog/posts/2022/11/%E7%BE%A4%E6%99%96%20btrfs%20%E5%85%B3%E4%BA%8E%20cow%20%E7%9A%84%E8%A7%82%E5%AF%9F/4.png?x-oss-process=style/ImageCompress)

![](https://img.mitsea.com/blog/posts/2022/11/%E7%BE%A4%E6%99%96%20btrfs%20%E5%85%B3%E4%BA%8E%20cow%20%E7%9A%84%E8%A7%82%E5%AF%9F/5.png?x-oss-process=style/ImageCompress)

## 应用

这个特性可以免去手动做硬链接的过程，在保种的时候也比较实用。

通常情况下，保种的文件里面也有我们需要的文件，对于这些文件需要放到其他文件夹里。此时就可以利用这一特性，直接复制文件即可。

![](https://img.mitsea.com/blog/posts/2022/11/%E7%BE%A4%E6%99%96%20btrfs%20%E5%85%B3%E4%BA%8E%20cow%20%E7%9A%84%E8%A7%82%E5%AF%9F/6.png?x-oss-process=style/ImageCompress)

比如这两个文件，左侧是在保种的文件夹里，右侧是从保种文件夹复制出来的，在自己整理的文件夹里。这两个文件 md5 一致，那就无需担心复制造成的重复空间占用。

## 疑惑

测试过程中也发现该特性似乎对文件夹来说是透明的，在统计文件夹大小的时候，仍然会重复计算文件大小

![](https://img.mitsea.com/blog/posts/2022/11/%E7%BE%A4%E6%99%96%20btrfs%20%E5%85%B3%E4%BA%8E%20cow%20%E7%9A%84%E8%A7%82%E5%AF%9F/7.png?x-oss-process=style/ImageCompress)

在同样支持 cow 的 APFS 中也表现出相同的行为，复制出来的副本虽然实际上不会额外占用空间，但是统计文件夹大小的时候仍然会重复统计。

![](https://img.mitsea.com/blog/posts/2022/11/%E7%BE%A4%E6%99%96%20btrfs%20%E5%85%B3%E4%BA%8E%20cow%20%E7%9A%84%E8%A7%82%E5%AF%9F/8.png?x-oss-process=style/ImageCompress)

> Photo by [Lysander Yuen](https://unsplash.com/@lysanderyuen?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/duplicate?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
