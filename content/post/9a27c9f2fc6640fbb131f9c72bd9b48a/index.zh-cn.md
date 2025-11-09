+++
author = "FlintyLemming"
title = "iPhone 6s 加厚电池充放电测试"
slug = "9a27c9f2fc6640fbb131f9c72bd9b48a"
date = "2020-06-01"
description = ""
categories = ["Apple", "Consumer"]
tags = ["iPhone", "改装"]
image = "https://assets.mitsea.cn/blog/posts/2020/06/iPhone%206s%20%E5%8A%A0%E5%8E%9A%E7%94%B5%E6%B1%A0%E5%85%85%E6%94%BE%E7%94%B5%E6%B5%8B%E8%AF%95/title.avif"
+++

之前在淘宝买了个 6s 的加厚电池，用了一段时间感觉确实不错，至少能一天一充了（原来重度使用的话一天两充都不够）。所以最近就做个充放电的测试，看看到底是个什么水平。

首先说明前置条件：
soc 代工厂是台积电
插卡，蜂窝数据打开（全程信号4G“满格”），偶尔来一两个广告电话，直接挂断
链接 Wi-Fi （使用PD充电时，连接2.4G Wi-Fi，信号“两格”，之后的测试均连接5G Wi-Fi，信号“满格”）
蓝牙打开，并几乎全程连接 Apple Watch S1（带着手表出去吃饭、运动未带手机）
正常接收重要通知（主要为IM软件）
开启自带邮件 app 接收（5个邮箱账号）
开启定位权限
另外开启了 iCloud 全部功能（包括家庭组）
系统为 iOS 12 正式版

## 充电
**使用 PD 充电套装充电**
PD充电器：>30w 输出功率的标准 PD 充电器
数据线：苹果官网 lighting - type-c 数据线（新款）
由于本人时间精力有限，没有做每分钟电量表格，所以充电情况直接看下图

![](https://assets.mitsea.cn/blog/posts/2020/06/iPhone%206s%20%E5%8A%A0%E5%8E%9A%E7%94%B5%E6%B1%A0%E5%85%85%E6%94%BE%E7%94%B5%E6%B5%8B%E8%AF%95/1.avif)

充电时间：23:00-2:40（大约），总时长大约为3小时40分钟，充电速度不快。

**使用标准 USB-A 2.1A 充电器（iPad 充电器）**
结果相同，故不再放图。全程基本稳定在 4.9V 1.2A 左右。


## 放电（使用 Geekbench 4 的电池测试）
**开启 “dim screen”，开启自动亮度（这里其实是忘记关了，不过由于测试过程在深夜，所以亮度应该是最低了）**
得分为：4223，其他设备得分也可看到。

![](https://assets.mitsea.cn/blog/posts/2020/06/iPhone%206s%20%E5%8A%A0%E5%8E%9A%E7%94%B5%E6%B1%A0%E5%85%85%E6%94%BE%E7%94%B5%E6%B5%8B%E8%AF%95/2.avif)

表现还是比较优异的，不过注意这是保持最低亮度的情况下。
详细结果：https://browser.geekbench.com/v4/battery/87828

**关闭“dim screen”，亮度为30%，关闭自动亮度调整**
得分为：4114，表现仍然相当不错。

![](https://assets.mitsea.cn/blog/posts/2020/06/iPhone%206s%20%E5%8A%A0%E5%8E%9A%E7%94%B5%E6%B1%A0%E5%85%85%E6%94%BE%E7%94%B5%E6%B5%8B%E8%AF%95/3.avif)

详细结果：https://browser.geekbench.com/v4/battery/87826

## 对比数据
https://www.youtube.com/watch?v=lkyy6WSHnGs&t=34s

![](https://assets.mitsea.cn/blog/posts/2020/06/iPhone%206s%20%E5%8A%A0%E5%8E%9A%E7%94%B5%E6%B1%A0%E5%85%85%E6%94%BE%E7%94%B5%E6%B5%8B%E8%AF%95/4.avif)

视频同样和上面一个测试相同，30%的亮度，关闭自动亮度，打开Wi-Fi
而且视频里的测试手机并没有插卡，所以得分应该比正常情况更高

## 总结
这款 iPhone 6s 加厚电池卖家标称 3900mAh，从分数上与原装电池（1715mAh）基本没有虚标，续航情况令人满意。但是充电速度不是很令人满意，充电的话还是中午或者晚上充比较合适。