+++
author = "FlintyLemming"
title = "iCloud 三个会无意占用额外空间的功能"
slug = "9fa2203c89274a7399c7b68625dce118"
date = "2024-08-22"
description = ""
categories = ["Apple"]
tags = ["iOS", "iCloud"]
image = "https://img.flinty.moe/blog/posts/2024/08/iCloud%20%E4%B8%89%E4%B8%AA%E4%BC%9A%E6%97%A0%E6%84%8F%E5%8D%A0%E7%94%A8%E9%A2%9D%E5%A4%96%E7%A9%BA%E9%97%B4%E7%9A%84%E5%8A%9F%E8%83%BD/steve-johnson-DY_NMDmcliM-unsplash.avif"
+++

# 云端信息

## 存在的问题

![](https://img.flinty.moe/blog/posts/2024/08/iCloud%20%E4%B8%89%E4%B8%AA%E4%BC%9A%E6%97%A0%E6%84%8F%E5%8D%A0%E7%94%A8%E9%A2%9D%E5%A4%96%E7%A9%BA%E9%97%B4%E7%9A%84%E5%8A%9F%E8%83%BD/image.avif)

这个其实是默认关闭的，他有一个问题，就是你在设备上的删除操作是不会同步到云端的。换句话说，他会无限增量存储你的信息。

你唯一可以部分删除云端信息的途径，就是图上有个管理存储空间，然后里面有个“最占空间的对话”，但是这只能删除掉一部分

![](https://img.flinty.moe/blog/posts/2024/08/iCloud%20%E4%B8%89%E4%B8%AA%E4%BC%9A%E6%97%A0%E6%84%8F%E5%8D%A0%E7%94%A8%E9%A2%9D%E5%A4%96%E7%A9%BA%E9%97%B4%E7%9A%84%E5%8A%9F%E8%83%BD/image%201.avif)

## 解决方法

如果这个功能你用不上，你可以直接关掉，你可以使用 iMazing 这种工具定期备份一下手机就行

如果你想只删除云端存储的部分信息，目前没有任何方法可以操作。但是有一个笨办法：

1. iCloud 里停用此功能并释放空间（这不会影响你本地的信息）
2. 删除自己不需要的信息
3. 等待 30 天可恢复期
4. 重新打开 iCloud 云端信息

但是如果你有聊天记录很多的 iMessage 对话，这个方法就不适用了，因为 iMessage 通常不会在手机本地保存全量的信息（你可以看下 信息 app 占用的空间，和云端空间对比一下）。此时如果你想要在关闭云同步前导出 iMessage 信息应该怎么办呢？很遗憾，就我的观察，最好的方法是使用 Mac。因为 Mac 开启 iMessage 信息云端同步后，是会全量下载 iMessage 聊天记录的，我本地有一个最早 2018 年的聊天，他都是全量同步到本地的。然后你再用比如 [imessage-exporter](https://github.com/ReagentX/imessage-exporter) 这样的工具导出来。

# iCloud 云盘

## 存在的问题

打开你手机的 文件 App，右下角的 浏览 里回到最顶层，你会看到 **我的 iPhone** 和 **iCloud 云盘** 两个项目，存在上面的内容不占云空间，存在下面的内容占用云空间。

![](https://img.flinty.moe/blog/posts/2024/08/iCloud%20%E4%B8%89%E4%B8%AA%E4%BC%9A%E6%97%A0%E6%84%8F%E5%8D%A0%E7%94%A8%E9%A2%9D%E5%A4%96%E7%A9%BA%E9%97%B4%E7%9A%84%E5%8A%9F%E8%83%BD/IMG_8766.avif)

这里有一个坑点，就是 iPhone 所有（比如你用 Safari）下载的文件和 AirDrop 收到的文件都会存在 **iCloud 云盘** 的 **下载** 文件夹里。

![](https://img.flinty.moe/blog/posts/2024/08/iCloud%20%E4%B8%89%E4%B8%AA%E4%BC%9A%E6%97%A0%E6%84%8F%E5%8D%A0%E7%94%A8%E9%A2%9D%E5%A4%96%E7%A9%BA%E9%97%B4%E7%9A%84%E5%8A%9F%E8%83%BD/IMG_8767.avif)

## 解决方法

**iCloud 云盘** 里的文件夹都检查检查是否是自己需要的文件，特别是 macOS 用户开了同步 桌面与文稿文件夹 的，注意看有没有软件在这里产生缓存文件和其他已经没用的文件。

# iCloud 云备份

## 存在的问题

这个问题就大了。首先你所有 App 都是默认开启了这个功能，这会导致一个现象：你的游戏资源文件、App 产生的各种缓存都会备份。但其实这些数据即便我们手机丢失后，也是可以重新下载的，完全不需要备份到 iCloud 占用空间。

![](https://img.flinty.moe/blog/posts/2024/08/iCloud%20%E4%B8%89%E4%B8%AA%E4%BC%9A%E6%97%A0%E6%84%8F%E5%8D%A0%E7%94%A8%E9%A2%9D%E5%A4%96%E7%A9%BA%E9%97%B4%E7%9A%84%E5%8A%9F%E8%83%BD/image%202.avif)

## 解决方法

手动关闭不需要备份的软件，基本上如果你要备份微信你就留个微信就行了，这个备份是全量备份微信所有数据，你手机丢了也能完整恢复到新手机里，对于需要的人来说还是非常好用的。

但是注意，你一旦新装了 App，要记得回到这里关闭同步。

另外，这里你关闭完全不影响软件内任何同步功能！包括使用 iCloud 同步数据的软件，可以放心关闭。因为控制这一功能的开关在 iCloud 第一页下面，不是这里。

![](https://img.flinty.moe/blog/posts/2024/08/iCloud%20%E4%B8%89%E4%B8%AA%E4%BC%9A%E6%97%A0%E6%84%8F%E5%8D%A0%E7%94%A8%E9%A2%9D%E5%A4%96%E7%A9%BA%E9%97%B4%E7%9A%84%E5%8A%9F%E8%83%BD/IMG_8768.avif)

一定要区分这两个地方的开关！