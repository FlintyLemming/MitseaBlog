+++
author = "FlintyLemming"
title = "A卡补帧 N卡输出"
slug = "8572fba5a37b459aab0f4a85bfe080b8"
date = "2019-12-12"
description = ""
categories = ["Consumer", "Windows"]
tags = ["显卡"]
image = "https://img.flinty.moe/blog/posts/2019/12/A%E5%8D%A1%E8%A1%A5%E5%B8%A7%20N%E5%8D%A1%E8%BE%93%E5%87%BA/title.avif"
+++

## 来源

[AN卡还能一起用？N卡输出解码，A卡补帧教程](https://www.bilibili.com/video/av9751675)

## 步骤

1. **将显示器接在 A 卡上**，打开 A 卡驱动控制面板，找到 视频，选择 Custom，打开 AMD Fluid Motion Video

    ![](https://img.flinty.moe/blog/posts/2019/12/A%E5%8D%A1%E8%A1%A5%E5%B8%A7%20N%E5%8D%A1%E8%BE%93%E5%87%BA/1.avif)

2. 下载并安装 Bluesky Frame Rate Converter，打开他的控制面板

    ![](https://img.flinty.moe/blog/posts/2019/12/A%E5%8D%A1%E8%A1%A5%E5%B8%A7%20N%E5%8D%A1%E8%BE%93%E5%87%BA/2.avif)

3. AFM Mode 改成 Mode 2，然后 Close

    ![](https://img.flinty.moe/blog/posts/2019/12/A%E5%8D%A1%E8%A1%A5%E5%B8%A7%20N%E5%8D%A1%E8%BE%93%E5%87%BA/3.avif)

4. 这时可以尝试看下有没有效果。打开 Potplayer 的设置，找到 滤镜 - 全局滤镜优先权

    ![](https://img.flinty.moe/blog/posts/2019/12/A%E5%8D%A1%E8%A1%A5%E5%B8%A7%20N%E5%8D%A1%E8%BE%93%E5%87%BA/4.avif)

5. 点击 添加系统滤镜… 找到 Bluesky Frame Rate Converter，选中，点击确定

    ![](https://img.flinty.moe/blog/posts/2019/12/A%E5%8D%A1%E8%A1%A5%E5%B8%A7%20N%E5%8D%A1%E8%BE%93%E5%87%BA/5.avif)

6. 选中刚才添加的这个滤镜，然后将 优先顺序 改为 强制使用

    ![](https://img.flinty.moe/blog/posts/2019/12/A%E5%8D%A1%E8%A1%A5%E5%B8%A7%20N%E5%8D%A1%E8%BE%93%E5%87%BA/6.avif)

7. 找到 滤镜 - 视频解码器，点击 内置解码器/DXVA设置

    ![](https://img.flinty.moe/blog/posts/2019/12/A%E5%8D%A1%E8%A1%A5%E5%B8%A7%20N%E5%8D%A1%E8%BE%93%E5%87%BA/7.avif)

8. 勾选 使用硬件加速

    ![](https://img.flinty.moe/blog/posts/2019/12/A%E5%8D%A1%E8%A1%A5%E5%B8%A7%20N%E5%8D%A1%E8%BE%93%E5%87%BA/8.avif)

9. 可以看到已经生效

    ![](https://img.flinty.moe/blog/posts/2019/12/A%E5%8D%A1%E8%A1%A5%E5%B8%A7%20N%E5%8D%A1%E8%BE%93%E5%87%BA/9.avif)

10. 将显示器接在 N 卡上，测试下，发现也可以正常补帧

    ![](https://img.flinty.moe/blog/posts/2019/12/A%E5%8D%A1%E8%A1%A5%E5%B8%A7%20N%E5%8D%A1%E8%BE%93%E5%87%BA/10.avif)

    看下占用，也没问题

    ![](https://img.flinty.moe/blog/posts/2019/12/A%E5%8D%A1%E8%A1%A5%E5%B8%A7%20N%E5%8D%A1%E8%BE%93%E5%87%BA/11.avif)
