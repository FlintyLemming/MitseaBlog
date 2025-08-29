+++
author = "FlintyLemming"
title = "老版本 Xcode 在新 iOS 设备上调试"
slug = "9da8a85a555d4d189f08024356e99f90"
date = "2019-10-06"
description = "主要就是补充 DeviceSupport"
categories = ["Apple"]
tags = ["Xcode", "iOS"]
image = "https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2019/10/%E8%80%81%E7%89%88%E6%9C%AC%20Xcode%20%E5%9C%A8%E6%96%B0%20iOS%20%E8%AE%BE%E5%A4%87%E4%B8%8A%E8%B0%83%E8%AF%95/title.avif"
+++

## 补充老版本 Xcode 缺失的 DeviceSupport
如果你有安装新版本的 Xcode，那可以直接拷贝过来，没有的话，可以去这里下载 [https://github.com/filsv/iPhoneOSDeviceSupport](https://github.com/filsv/iPhoneOSDeviceSupport) 。下载完毕后，拷贝到 Xcode/Contents/Developer/Platforms/iPhoneOS.platform/DeviceSupport 目录下，当然也可以加上具体的版本号，以 iOS 13.1.1 为例，文件夹名称就可以写为

```
13.1.1 (17A854)
```

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2019/10/%E8%80%81%E7%89%88%E6%9C%AC%20Xcode%20%E5%9C%A8%E6%96%B0%20iOS%20%E8%AE%BE%E5%A4%87%E4%B8%8A%E8%B0%83%E8%AF%95/1.avif)

其实补充 DeviceSupport 就可以解决一部分一个大版本内无法调试的情况。如果跨大版本，请继续下面的操作

## 修改 SDK 文件信息

在 Xcode/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs 目录下，复制一份现有的 sdk 文件，并修改为需要的版本，比如我这里就修改为 iPhoneOS13.1.1.sdk

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2019/10/%E8%80%81%E7%89%88%E6%9C%AC%20Xcode%20%E5%9C%A8%E6%96%B0%20iOS%20%E8%AE%BE%E5%A4%87%E4%B8%8A%E8%B0%83%E8%AF%95/2.avif)

然后 修改 iPhoneOS.sdk 里（不是 iPhoneOS13.1.1.sdk）的 SDKSettings.plist 文件。将原来的版本信息全部改成你需要的，比如我这里都改成 13.1.1。当然那个 DefaultDeploymentTarget 仍然可以按照原来的格式改成 13.1.99。改完保存，可能关闭按钮上显示没有保存，但其实是保存了的。如果不能保存，把文件拖到别的地方，改好后再覆盖回去。

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2019/10/%E8%80%81%E7%89%88%E6%9C%AC%20Xcode%20%E5%9C%A8%E6%96%B0%20iOS%20%E8%AE%BE%E5%A4%87%E4%B8%8A%E8%B0%83%E8%AF%95/3.avif)

## 重启 Xcode
一定要重启，不然不会生效