+++
author = "FlintyLemming"
title = "小米 4 Project Treble 过程"
slug = "76a6f0299d5141be86b3f9c50cc8429e"
date = "2020-01-19"
description = ""
categories = ["Android"]
tags = ["Project Treble", "小米"]
image = "https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/01/%E5%B0%8F%E7%B1%B3%204%20Project%20Treble%20%E8%BF%87%E7%A8%8B/title.avif"
+++

原帖来自 [https://forum.xda-developers.com/xiaomi-mi-3/development/guide-how-to-create-vendor-to-flash-gsi-t3828310](https://forum.xda-developers.com/xiaomi-mi-3/development/guide-how-to-create-vendor-to-flash-gsi-t3828310)

1. 进入 fastboot 刷入支持 Project Treble 的 recovery
文件链接：[https://drive.google.com/file/d/1Fc0SK_xYdmkFi5CzR4Vq8QLqQFQ1ikWk/view?usp=sharing](https://drive.google.com/file/d/1Fc0SK_xYdmkFi5CzR4Vq8QLqQFQ1ikWk/view?usp=sharing)
2. 刷入 Treble 化的包
项目地址：[https://github.com/cjybyjk/cancro_treblizer/releases](https://github.com/cjybyjk/cancro_treblizer/releases)
文件链接：[https://drive.google.com/file/d/14EkMX9VKiC3kbELwtBkxmAYupCmCxPf5/view?usp=sharing](https://drive.google.com/file/d/14EkMX9VKiC3kbELwtBkxmAYupCmCxPf5/view?usp=sharing)
3. 格式化 Vendor 分区：make_ext4fs /dev/block/bootdevice/by-name/vendor
4. 同理格式化 Cache 分区
5. 刷入一个基础系统，比如los的这个包：[https://androidfilehost.com/?fid=3700668719832236822](https://androidfilehost.com/?fid=3700668719832236822)
然后就可以刷入 Project Treble GIS 包了
刷的时候注意一下，你下的一般是个 .xz 的压缩包，解压出来获得 .img 文件，然后是进 fastboot 刷，因为小米 4 是 32 位设备，所以不存在 system_a、b 之类，直接刷 system 分区就行
