+++
author = "FlintyLemming"
title = "老平台迁移系统至 nvme SSD 并启动"
slug = "9fd5b88b432442358afb90469768679b"
date = "2022-11-27"
description = ""
categories = ["Windows", "HomeLab", "LifeTec"]
tags = ["引导", "Windows"]
image = "https://hf-image.mitsea.com:8840/blog/posts/2022/11/%E8%80%81%E5%B9%B3%E5%8F%B0%E8%BF%81%E7%A7%BB%E7%B3%BB%E7%BB%9F%E8%87%B3%20nvme%20SSD%20%E5%B9%B6%E5%90%AF%E5%8A%A8/philip-oroni-DsKJeR1DJxc-unsplash.avif"
+++

公司里电脑比较老，是 Dell 的 Optiplex 790，2 代 i5。硬盘是很慢的机械硬盘，手头正好有个闲置的 SN750 256G，打算迁移系统到这个 SSD 上。

但是老平台不支持从 nvme 启动，所以除了迁移还需要其他的操作。这次主要干了两件事：借助 Clover 启动 nvme 硬盘里的系统；解决 Windows 从 SATA HDD 迁移到 nvme SSD 后无法启动的问题。

本来这种老电脑升级硬盘应该是用 SATA SSD，快捷方便，但是经常 SATA SSD 比 nvme SSD 还贵，所以这么操作还是有一定价值的。

![](https://hf-image.mitsea.com:8840/blog/posts/2022/11/%E8%80%81%E5%B9%B3%E5%8F%B0%E8%BF%81%E7%A7%BB%E7%B3%BB%E7%BB%9F%E8%87%B3%20nvme%20SSD%20%E5%B9%B6%E5%90%AF%E5%8A%A8/Untitled.avif)

## 迁移系统

把 nvme 通过转接板插到电脑上后，用你所熟悉的任何工具把原来硬盘的所有分区搬到新硬盘上。我是用的 Ghost 先备份后再还原到新硬盘上的，这样万一折腾坏了还能再还原。

迁移好后把原来硬盘里的分区都删了，最后大概就是这样。第一块是原来的 HDD，第二块是追加的 SSD。

![](https://hf-image.mitsea.com:8840/blog/posts/2022/11/%E8%80%81%E5%B9%B3%E5%8F%B0%E8%BF%81%E7%A7%BB%E7%B3%BB%E7%BB%9F%E8%87%B3%20nvme%20SSD%20%E5%B9%B6%E5%90%AF%E5%8A%A8/Untitled.avif)

## 处理原系统

首先要检查下迁移完后的 BCD 是否正确指向新分区。步骤如下

1. 打开 BOOTICE
2. 打开 BCD 编辑 选项卡，选择已经在 SSD 里的 EFI 分区中的 BCD 文件后（在 EFI\Microsoft\Boot 里），用 智能编辑模式 打开。
3. 这里检查下是不是 SSD 里的系统盘
    
    ![](https://hf-image.mitsea.com:8840/blog/posts/2022/11/%E8%80%81%E5%B9%B3%E5%8F%B0%E8%BF%81%E7%A7%BB%E7%B3%BB%E7%BB%9F%E8%87%B3%20nvme%20SSD%20%E5%B9%B6%E5%90%AF%E5%8A%A8/6.avif)
    

因为之前是 SATA 启动，所以系统引导时不会从 nvme 硬盘里找系统，会报一个 **INACCESSIBLE_BOOT_DEVICE** 的错误。我们要把之前留下的设置给删掉，步骤如下

1. 打开注册表编辑器
2. 点选 HKEY_LOCAL_MACHINE 后，点击 文件 - 加载配置单元
    
    ![](https://hf-image.mitsea.com:8840/blog/posts/2022/11/%E8%80%81%E5%B9%B3%E5%8F%B0%E8%BF%81%E7%A7%BB%E7%B3%BB%E7%BB%9F%E8%87%B3%20nvme%20SSD%20%E5%B9%B6%E5%90%AF%E5%8A%A8/7.avif)
    
3. 这里选择打开 SSD 里系统盘 Windows\System32\config\SYSTEM 文件
    
    ![](https://hf-image.mitsea.com:8840/blog/posts/2022/11/%E8%80%81%E5%B9%B3%E5%8F%B0%E8%BF%81%E7%A7%BB%E7%B3%BB%E7%BB%9F%E8%87%B3%20nvme%20SSD%20%E5%B9%B6%E5%90%AF%E5%8A%A8/8.avif)
    
4. 名字随便写
    
    ![](https://hf-image.mitsea.com:8840/blog/posts/2022/11/%E8%80%81%E5%B9%B3%E5%8F%B0%E8%BF%81%E7%A7%BB%E7%B3%BB%E7%BB%9F%E8%87%B3%20nvme%20SSD%20%E5%B9%B6%E5%90%AF%E5%8A%A8/9.avif)
    
5. 找到 ControlSet001\Services\stornvme\StartOverride，把他删掉
    
    ![](https://hf-image.mitsea.com:8840/blog/posts/2022/11/%E8%80%81%E5%B9%B3%E5%8F%B0%E8%BF%81%E7%A7%BB%E7%B3%BB%E7%BB%9F%E8%87%B3%20nvme%20SSD%20%E5%B9%B6%E5%90%AF%E5%8A%A8/10.avif)
    
6. 删完后，还选择刚才挂载的文件夹，比如我这里是 fix。然后 文件 - 卸载配置单元。
    
    ![](https://hf-image.mitsea.com:8840/blog/posts/2022/11/%E8%80%81%E5%B9%B3%E5%8F%B0%E8%BF%81%E7%A7%BB%E7%B3%BB%E7%BB%9F%E8%87%B3%20nvme%20SSD%20%E5%B9%B6%E5%90%AF%E5%8A%A8/11.avif)
    

## 配置 Clover

Clover 是一个引导工具，装黑苹果用的比较多。不过它即便仅作为多系统环境引导也很好用。本次就需要借助 Clover 来启动 nvme 里的系统。

OpenCore 应该也可以，但是 OpenCore 需要先编辑好一个正确的配置文件才能用，比较麻烦。Clover 不需要特别去配置 config，直接用就行。

1. 从[这里](https://github.com/CloverHackyColor/CloverBootloader/releases/)下载 Clover。下这个 zip 文件，版本号可能不一样，但一般就是最大的这个 zip 文件。
    
    ![](https://hf-image.mitsea.com:8840/blog/posts/2022/11/%E8%80%81%E5%B9%B3%E5%8F%B0%E8%BF%81%E7%A7%BB%E7%B3%BB%E7%BB%9F%E8%87%B3%20nvme%20SSD%20%E5%B9%B6%E5%90%AF%E5%8A%A8/Untitled%201.avif)
    
2. 解压后，为了让 Clover 能识别到 nvme 硬盘，需要把这里的 NvmExpressDxe.efi 驱动挪到 drivers 的 UEFI 文件夹里
    
    ![](https://hf-image.mitsea.com:8840/blog/posts/2022/11/%E8%80%81%E5%B9%B3%E5%8F%B0%E8%BF%81%E7%A7%BB%E7%B3%BB%E7%BB%9F%E8%87%B3%20nvme%20SSD%20%E5%B9%B6%E5%90%AF%E5%8A%A8/Untitled%202.avif)
    
3. 对原机械硬盘执行快速格式化，分区选一个就行。这样我们就获得了一个 EFI 分区和一个 NTFS 主分区。前者拿来装 Clover，后者之后进系统可以拿来放放文件。
    
    ![](https://hf-image.mitsea.com:8840/blog/posts/2022/11/%E8%80%81%E5%B9%B3%E5%8F%B0%E8%BF%81%E7%A7%BB%E7%B3%BB%E7%BB%9F%E8%87%B3%20nvme%20SSD%20%E5%B9%B6%E5%90%AF%E5%8A%A8/2.avif)
    
4. 点击 EFI 分区，然后查看文件。把刚才准备的 Clover 里这个 EFI 文件拖进去
    
    ![](https://hf-image.mitsea.com:8840/blog/posts/2022/11/%E8%80%81%E5%B9%B3%E5%8F%B0%E8%BF%81%E7%A7%BB%E7%B3%BB%E7%BB%9F%E8%87%B3%20nvme%20SSD%20%E5%B9%B6%E5%90%AF%E5%8A%A8/Untitled%203.avif)
    
5. 为了避免老主板加载启动项的时候识别不出来这个 EFI 分区，最好再去 BOOTICE 里把 Clover 加上。
    
    ![](https://hf-image.mitsea.com:8840/blog/posts/2022/11/%E8%80%81%E5%B9%B3%E5%8F%B0%E8%BF%81%E7%A7%BB%E7%B3%BB%E7%BB%9F%E8%87%B3%20nvme%20SSD%20%E5%B9%B6%E5%90%AF%E5%8A%A8/4.avif)
    

## 启动系统

启动项里可以看到刚才添加的 Clover

![](https://hf-image.mitsea.com:8840/blog/posts/2022/11/%E8%80%81%E5%B9%B3%E5%8F%B0%E8%BF%81%E7%A7%BB%E7%B3%BB%E7%BB%9F%E8%87%B3%20nvme%20SSD%20%E5%B9%B6%E5%90%AF%E5%8A%A8/35E8125C-C342-4FA1-818F-4873DE188651_1_105_c.avif)

进到 Clover 后，就可以看到 Windows 引导项目了，一般就是第一个 Boot Microsoft EFI Boot from EFI

![](https://hf-image.mitsea.com:8840/blog/posts/2022/11/%E8%80%81%E5%B9%B3%E5%8F%B0%E8%BF%81%E7%A7%BB%E7%B3%BB%E7%BB%9F%E8%87%B3%20nvme%20SSD%20%E5%B9%B6%E5%90%AF%E5%8A%A8/5351610F-C865-49EB-AF2C-2D7E6EA3029F_1_105_c.avif)

成功启动

![](https://hf-image.mitsea.com:8840/blog/posts/2022/11/%E8%80%81%E5%B9%B3%E5%8F%B0%E8%BF%81%E7%A7%BB%E7%B3%BB%E7%BB%9F%E8%87%B3%20nvme%20SSD%20%E5%B9%B6%E5%90%AF%E5%8A%A8/B34D3A72-FDE8-4CD1-B266-349D57BBBD6E_1_105_c.avif)

> Photo by [Philip Oroni](https://unsplash.com/@philipsfuture?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)