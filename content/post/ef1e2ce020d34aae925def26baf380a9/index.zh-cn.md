+++
author = "FlintyLemming"
title = "提升 Windows 远程桌面的音频质量"
slug = "ef1e2ce020d34aae925def26baf380a9"
date = "2020-01-21"
description = ""
categories = ["Windows"]
tags = ["远程桌面"]
image = "https://img.flinty.moe/blog/posts/2020/01/%E6%8F%90%E5%8D%87%20Windows%20%E8%BF%9C%E7%A8%8B%E6%A1%8C%E9%9D%A2%E7%9A%84%E9%9F%B3%E9%A2%91%E8%B4%A8%E9%87%8F/title.avif"
+++

由于音频质量是否有提升主观因素较大，所以并不能保证实际上有用。此外，设置后，远程桌面的操作可能比以往卡顿，谨慎使用

## 被控端（服务端）设置

1. 通过 Windows 10 的搜索或者 运行 功能，搜索并打开 gpedit.msc，组策略

    ![](https://img.flinty.moe/blog/posts/2020/01/%E6%8F%90%E5%8D%87%20Windows%20%E8%BF%9C%E7%A8%8B%E6%A1%8C%E9%9D%A2%E7%9A%84%E9%9F%B3%E9%A2%91%E8%B4%A8%E9%87%8F/1.avif)

2. 依次找到 计算机配置 - 管理模板 - Windows 组件 - 远程桌面服务 - 远程桌面会话主机 - 设备和资源重定向

    ![](https://img.flinty.moe/blog/posts/2020/01/%E6%8F%90%E5%8D%87%20Windows%20%E8%BF%9C%E7%A8%8B%E6%A1%8C%E9%9D%A2%E7%9A%84%E9%9F%B3%E9%A2%91%E8%B4%A8%E9%87%8F/2.avif)

3. 双击打开 限制音频播放质量，打开 已启用，然后音频质量选择 高

    ![](https://img.flinty.moe/blog/posts/2020/01/%E6%8F%90%E5%8D%87%20Windows%20%E8%BF%9C%E7%A8%8B%E6%A1%8C%E9%9D%A2%E7%9A%84%E9%9F%B3%E9%A2%91%E8%B4%A8%E9%87%8F/3.avif)

## 控制端（客户端）配置

1. 编辑 rdp 配置文件，macOS 好像没有直接编辑的地方，我这里是先导出

    ![](https://img.flinty.moe/blog/posts/2020/01/%E6%8F%90%E5%8D%87%20Windows%20%E8%BF%9C%E7%A8%8B%E6%A1%8C%E9%9D%A2%E7%9A%84%E9%9F%B3%E9%A2%91%E8%B4%A8%E9%87%8F/4.avif)

    Windows 的话，在 显示 选项卡这里有个打开

    ![](https://img.flinty.moe/blog/posts/2020/01/%E6%8F%90%E5%8D%87%20Windows%20%E8%BF%9C%E7%A8%8B%E6%A1%8C%E9%9D%A2%E7%9A%84%E9%9F%B3%E9%A2%91%E8%B4%A8%E9%87%8F/5.avif)

2. 然后编辑导出的 .rdp 文件，在最后加一句

        set audioqualitymode:i:2

    ![](https://img.flinty.moe/blog/posts/2020/01/%E6%8F%90%E5%8D%87%20Windows%20%E8%BF%9C%E7%A8%8B%E6%A1%8C%E9%9D%A2%E7%9A%84%E9%9F%B3%E9%A2%91%E8%B4%A8%E9%87%8F/6.avif)

3. 重新连接即可

## 其他配置

关于远程桌面其他性能设置，可以参考微软的这篇 Wiki （English）

[Performance Tuning Remote Desktop Session Hosts](https://docs.microsoft.com/en-us/windows-server/administration/performance-tuning/role/remote-desktop/session-hosts)