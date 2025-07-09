+++
author = "FlintyLemming"
title = "【归档】iOS 解除温控降频"
slug = "63dd78918fa6484790bb04891111f429"
date = "2020-03-08"
description = ""
categories = ["Apple"]
tags = ["iOS"]
image = "https://img.mitsea.com/blog/posts/2020/03/iOS%20%E8%A7%A3%E9%99%A4%E6%B8%A9%E6%8E%A7%E9%99%8D%E9%A2%91/title.avif"
+++

**本文仅限于越狱系统，并且修改过后运行大型应用设备会异常的烫，耗电也会异常的快**

本文提供两种方法，包括一键修改和手动修改，我个人是推荐手动修改。你觉得哪个稳就用哪个方法。

# 插件一键修改

越狱商店搜索 Apple Turbo Boost，安装即可

![](https://img.mitsea.com/blog/posts/2020/03/iOS%20%E8%A7%A3%E9%99%A4%E6%B8%A9%E6%8E%A7%E9%99%8D%E9%A2%91/1.avif)

# 手动修改

1. 获取设备的 Model，可以在商店搜索 BMSSM 下载“Battery Memory System Status Monitor”，但是这个查看不了 CPU 的主频（显示错误），如果还需要看主频，下载“CPU DasherX”，这个是收费的。
2. 下载好后，这两个软件分别在这个位置可以看到设备 Model，记下来。

    ![](https://img.mitsea.com/blog/posts/2020/03/iOS%20%E8%A7%A3%E9%99%A4%E6%B8%A9%E6%8E%A7%E9%99%8D%E9%A2%91/2.avif)

    ![](https://img.mitsea.com/blog/posts/2020/03/iOS%20%E8%A7%A3%E9%99%A4%E6%B8%A9%E6%8E%A7%E9%99%8D%E9%A2%91/3.avif)

3. 用文件管理器定位到下面的目录，没用文件管理器的从越狱商店下载安装一个Filza

    ```plain
    /System/library/Watchdog/ThermalMonitor.bundle/<设备Model>.bundle
    ```

    打开里面的**info.plist**文件

    上面是 iOS 12 的路径，iOS 13 的路径如下

    ```plain
    /System/library/ThermalMonitor/<设备Model>.plist
    ```

    直接打开这个 plist 文件

4. 在/Root下看看有没有 thermalMitigationLimits 和 contextualClampParams 这两项，如果有，左划，删除，没有的话看下一步
5. 在/Root/hotspots 下会看到几个 Item。除了Item 0，从 Item 1 开始如果看到有以下内容，修改数值为99

    ```plain
    ForcedThermalLevelTarget0
    ForcedThermalLevelTarget1
    ForcedThermalPressureLevelLightTarget
    THERMAL_TRAP_LOAD
    THERMAL_TRAP_SLEEP
    target
    ```

6. 重启手机即刻生效。

> 参考：[https://m.feng.com/detail/post/12039894](https://m.feng.com/detail/post/12039894)