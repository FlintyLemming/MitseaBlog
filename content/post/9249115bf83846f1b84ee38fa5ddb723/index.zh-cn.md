+++
author = "FlintyLemming"
title = "Windows Server 2019 安装 Intel I219V 网卡驱动"
date = "2020-01-07"
description = ""
categories = ["Windows", "Network"]
tags = ["Windows", "10GbE"]
image = "https://img.mitsea.com/blog/posts/2020/01/Windows%20Server%202019%20%E5%AE%89%E8%A3%85%20Intel%20I219V%20%E7%BD%91%E5%8D%A1%E9%A9%B1%E5%8A%A8/title.jpg?x-oss-process=style/ImageCompress"
+++

1. 从主板官网下载用于 Windows 10 的网卡驱动

    ![](https://img.mitsea.com/blog/posts/2020/01/Windows%20Server%202019%20%E5%AE%89%E8%A3%85%20Intel%20I219V%20%E7%BD%91%E5%8D%A1%E9%A9%B1%E5%8A%A8/1.png?x-oss-process=style/ImageCompress)

    我这边下的是 23.0，并不是 23.5

2. 下载后解压，找到 Intel_lan/PRO1000/Winx64/NDIS65 文件夹，打开（这边建议拷贝这个文件夹到 Windows Server 2019 上继续操作）

    ![](https://img.mitsea.com/blog/posts/2020/01/Windows%20Server%202019%20%E5%AE%89%E8%A3%85%20Intel%20I219V%20%E7%BD%91%E5%8D%A1%E9%A9%B1%E5%8A%A8/2.png?x-oss-process=style/ImageCompress)

3. 对于 I217V, 218V 和 219V，需要修改 e1d65x64.inf 这个文件。而 I211 则是修改 e1r65x64.inf 这个。
    - 关于如何修改哪个文件，可以参考如下步骤
        1. 右键 Windows 徽标，打开设备管理器
        2. 在 其他设备 里找到 以太网控制器，双击打开
        3. 在 详细信息 里，属性切到 硬件 Id，可以看到这里有几个值

    ![](https://img.mitsea.com/blog/posts/2020/01/Windows%20Server%202019%20%E5%AE%89%E8%A3%85%20Intel%20I219V%20%E7%BD%91%E5%8D%A1%E9%A9%B1%E5%8A%A8/3.png?x-oss-process=style/ImageCompress)

        4. 比如我这里第一项是 PCI\VEN_8086&DEV_15B8&SUBSYS_E00001458&REV_00，那就去头舍尾，取如下字段

            ```bash
            VEN_8086&DEV_15B8
            ```

        5. 然后通过 PowerShell 快速检索

            ```bash
            Get-ChildItem -Path "<PRO1000\Winx64\NDIS65 文件夹的绝对路径>" ` -recurse | Select-String -pattern "VEN_8086&DEV_15B8" | group path | select name
            ```

            通过返回值可以看到我的网卡对应的驱动文件是 e1d65x64

    ![](https://img.mitsea.com/blog/posts/2020/01/Windows%20Server%202019%20%E5%AE%89%E8%A3%85%20Intel%20I219V%20%E7%BD%91%E5%8D%A1%E9%A9%B1%E5%8A%A8/4.png?x-oss-process=style/ImageCompress)

4. 找到 [ControlFlags] 部分，删除其中的内容，或者替换成 *，就像这样

    ![](https://img.mitsea.com/blog/posts/2020/01/Windows%20Server%202019%20%E5%AE%89%E8%A3%85%20Intel%20I219V%20%E7%BD%91%E5%8D%A1%E9%A9%B1%E5%8A%A8/5.png?x-oss-process=style/ImageCompress)

    或者这样

    ![](https://img.mitsea.com/blog/posts/2020/01/Windows%20Server%202019%20%E5%AE%89%E8%A3%85%20Intel%20I219V%20%E7%BD%91%E5%8D%A1%E9%A9%B1%E5%8A%A8/6.png?x-oss-process=style/ImageCompress)

5. 找到 [Intel.NTamd64.10.0.1] 部分，从第一项 %E153ANC.DeviceDesc% …… 开始，到最后，剪切到 [Intel.NTamd64.10.0] 的最后，就像这样

    ![](https://img.mitsea.com/blog/posts/2020/01/Windows%20Server%202019%20%E5%AE%89%E8%A3%85%20Intel%20I219V%20%E7%BD%91%E5%8D%A1%E9%A9%B1%E5%8A%A8/7.jpeg)

    改好后保存

6. Windows Server 2019 机器上，打开 设置 - 更新和安全 - 恢复 - 立即重新启动
7. 恢复模式下，选择 疑难解答 - 启动设置 - 重启
8. 在 高级启动选项 里，选择 禁用驱动强制签名
9. 重新系统后，打开 设备管理器 - 其他设备 - 双击以太网控制器 - 更新驱动程序 - 浏览我的计算机以查找驱动程序软件，选择刚才改好文件的那个 NDIS65 文件夹

    ![](https://img.mitsea.com/blog/posts/2020/01/Windows%20Server%202019%20%E5%AE%89%E8%A3%85%20Intel%20I219V%20%E7%BD%91%E5%8D%A1%E9%A9%B1%E5%8A%A8/8.png?x-oss-process=style/ImageCompress)

10. 在弹出的窗口中选择 始终安装此驱动程序软件

    ![](https://img.mitsea.com/blog/posts/2020/01/Windows%20Server%202019%20%E5%AE%89%E8%A3%85%20Intel%20I219V%20%E7%BD%91%E5%8D%A1%E9%A9%B1%E5%8A%A8/9.png?x-oss-process=style/ImageCompress)

11. Ok，有网了

    ![](https://img.mitsea.com/blog/posts/2020/01/Windows%20Server%202019%20%E5%AE%89%E8%A3%85%20Intel%20I219V%20%E7%BD%91%E5%8D%A1%E9%A9%B1%E5%8A%A8/10.png?x-oss-process=style/ImageCompress)
