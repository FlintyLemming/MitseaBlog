+++
author = "FlintyLemming"
title = "【归档】Android 开启导航栏"
slug = "2e0e821ebe6a42608ecc2e64a5130875"
date = "2020-05-28"
description = "导航按钮不灵的老手机可能会用到"
categories = ["Android"]
tags = []
image = "https://blog-img.mitsea.com/images/blog/posts/2020/05/Android%20%E5%BC%80%E5%90%AF%E5%AF%BC%E8%88%AA%E6%A0%8F/title.avif"
+++

导航按钮不灵的老手机可能会用到

```bash
adb shell
su
getprop qemu.hw.mainkeys
setprop qemu.hw.mainkeys 0
stop
start
```
