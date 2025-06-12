+++
author = "FlintyLemming"
title = "Galaxy Watch 与 Google Fit 的数据同步（含微信步数同步）"
slug = "e4cb02f80139426499e17d2203723492"
date = "2024-09-14"
description = ""
categories = ["Android", "Consumer"]
tags = ["wearOS", "Galaxy"]
image = "https://img.flinty.moe/blog/posts/2024/09/Galaxy%20Watch%20%E4%B8%8E%20Google%20Fit%20%E7%9A%84%E6%95%B0%E6%8D%AE%E5%90%8C%E6%AD%A5%EF%BC%88%E5%90%AB%E5%BE%AE%E4%BF%A1%E6%AD%A5%E6%95%B0%E5%90%8C%E6%AD%A5%EF%BC%89/mathias-reding-hC09pgro2yY-unsplash.avif"
+++

## 背景

由于各种原因，三星健康默认有的数据是不能很好的同步到 Google Fit 上的，这里针对不同类型的数据给出不同的解决方案。

方法中有的是自己摸索的，有的是网上看到的，不是纯原创。

## 具体方法

### 健身记录

如果用 Galaxy Watch 自带的跑步程序的话，记录的数据同步到 Google Fit 后有可能没有心肺强化分数

![](https://img.flinty.moe/blog/posts/2024/09/Galaxy%20Watch%20%E4%B8%8E%20Google%20Fit%20%E7%9A%84%E6%95%B0%E6%8D%AE%E5%90%8C%E6%AD%A5%EF%BC%88%E5%90%AB%E5%BE%AE%E4%BF%A1%E6%AD%A5%E6%95%B0%E5%90%8C%E6%AD%A5%EF%BC%89/Screenshot_20240912_232045_Fit.avif)

你需要在手表已经刷了美版固件的情况下，去 Play 商店下载 Google Fit，然后你的应用列表里就会多出来几个 App，以后跑步的时候直接用这个就可以（蓝色小人的那个 app）。不过由于用的是 Google Map，定位肯定是飘的，这个没什么办法

![](https://img.flinty.moe/blog/posts/2024/09/Galaxy%20Watch%20%E4%B8%8E%20Google%20Fit%20%E7%9A%84%E6%95%B0%E6%8D%AE%E5%90%8C%E6%AD%A5%EF%BC%88%E5%90%AB%E5%BE%AE%E4%BF%A1%E6%AD%A5%E6%95%B0%E5%90%8C%E6%AD%A5%EF%BC%89/STIIITCH_2024_09_12_23_26_22.avif)

### 步数（微信步数同步适用）

若要将三星健康的数据同步到 Google Fit，需要在三星健康的设置里打开 健康连接，然后全部允许数据。这个 健康连接 就是 Google 自己的 Health Kit，但是默认情况下你会发现三星健康是不给 Google 步数数据的

![](https://img.flinty.moe/blog/posts/2024/09/Galaxy%20Watch%20%E4%B8%8E%20Google%20Fit%20%E7%9A%84%E6%95%B0%E6%8D%AE%E5%90%8C%E6%AD%A5%EF%BC%88%E5%90%AB%E5%BE%AE%E4%BF%A1%E6%AD%A5%E6%95%B0%E5%90%8C%E6%AD%A5%EF%BC%89/IMG_8873.avif)

这是因为三星健康在大陆是不会将步数数据提供给任何第三方软件的，微信步数也不行，需要通过以下手段绕过限制：

1. 在手机根目录下的 Download 文件夹里创建 SamsungHealth 文件夹
2. 在这个 SamsungHealth 文件夹里创建一个名为 FeatureManagerOn 的无后缀文件
3. 打开三星健康的设置 - 关于三星健康 - 连击版本号打开开发者选项
4. 打开 Set features，修改 HealthAnalytics 下的 Server 为 DEV，修改 DataPlatform 里的 Developer mode 为 on
    
    ![](https://img.flinty.moe/blog/posts/2024/09/Galaxy%20Watch%20%E4%B8%8E%20Google%20Fit%20%E7%9A%84%E6%95%B0%E6%8D%AE%E5%90%8C%E6%AD%A5%EF%BC%88%E5%90%AB%E5%BE%AE%E4%BF%A1%E6%AD%A5%E6%95%B0%E5%90%8C%E6%AD%A5%EF%BC%89/IMG_8877.avif)
    
5. 然后返回，app 会提示你强制停止 app，强制停止后重新运行就行
6. 这个时候重新回到 Health Connect 里面，就可以看到三星健康已经可以向 Google Fit 写入步数数据了
    
    ![](https://img.flinty.moe/blog/posts/2024/09/Galaxy%20Watch%20%E4%B8%8E%20Google%20Fit%20%E7%9A%84%E6%95%B0%E6%8D%AE%E5%90%8C%E6%AD%A5%EF%BC%88%E5%90%AB%E5%BE%AE%E4%BF%A1%E6%AD%A5%E6%95%B0%E5%90%8C%E6%AD%A5%EF%BC%89/Screenshot_20240912_234510_HealthConnect.avif)
    

### 睡眠

三星健康似乎无论如何也不会向 Google Fit 写入睡眠数据，这个时候就需要借助第三方 app 同步数据了，这里推荐一款健康数据同步工具

[Health Sync - Apps on Google Play](https://play.google.com/store/apps/details?id=nl.appyhapps.healthsync)

下载直接使用即可。注意，不能绕过上面破解大陆限制的步骤进行同步，否则这个 app 还是拿不到三星健康的数据

设置完后基本数据就都能正常同步了，心率和步数由 三星健康 App 提供，步数则由 Health Sync 提供

![](https://img.flinty.moe/blog/posts/2024/09/Galaxy%20Watch%20%E4%B8%8E%20Google%20Fit%20%E7%9A%84%E6%95%B0%E6%8D%AE%E5%90%8C%E6%AD%A5%EF%BC%88%E5%90%AB%E5%BE%AE%E4%BF%A1%E6%AD%A5%E6%95%B0%E5%90%8C%E6%AD%A5%EF%BC%89/Screenshot_20240912_235113_Fit.avif)

> Photo by [Mathias Reding](https://unsplash.com/@matreding?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/a-close-up-of-a-piece-of-art-with-paint-on-it-hC09pgro2yY?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)
  