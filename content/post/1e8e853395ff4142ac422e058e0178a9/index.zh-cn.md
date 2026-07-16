+++
author = "FlintyLemming"
title = "群晖 Active Backup for Business 恢复 Windows 设备"
slug = "1e8e853395ff4142ac422e058e0178a9"
date = "2022-09-26"
description = ""
categories = ["HomeLab", "Windows"]
tags = ["Synology", "NAS"]
image = "https://assets.flinty.moe/blog/posts/2022/09/%E7%BE%A4%E6%99%96%20Active%20Backup%20for%20Business%20%E6%81%A2%E5%A4%8D%20Windows%20%E8%AE%BE%E5%A4%87/title.avif"
+++

当使用 Active Backup for Business 备份一台机器的系统卷或者全部设备后，机器挂掉之后可以通过已有的备份来恢复。

## 注意点

在介绍恢复方法前，简要说一下 Active Backup for Business 恢复的一些特性和注意点。

1. 要恢复的硬盘大小必须大于等于原硬盘的大小
2. 需要准备一个U盘
3. 如果你还没有恢复镜像，你需要一个 Windows 的环境
4. 只要群晖有公网，群晖和恢复的机器可以不在同一个局域网内
5. 可以拿来迁移系统到其他设备，一般情况下都可以修复引导并正常启动

## 恢复方法

### 准备恢复镜像

<aside>
💡 如果你的U盘中装有 Ventoy，也可以直接从我这里下载打包好的恢复镜像并放到 Ventoy 目录下，启动时直接选择 iso 即可。

</aside>

1. NAS 中打开 Active Backup for Business，找到 还原 - 整台设备
    
    ![](https://assets.flinty.moe/blog/posts/2022/09/%E7%BE%A4%E6%99%96%20Active%20Backup%20for%20Business%20%E6%81%A2%E5%A4%8D%20Windows%20%E8%AE%BE%E5%A4%87/1.avif)
    
2. 下载这里第一个 恢复介质创建程序
    
    ![](https://assets.flinty.moe/blog/posts/2022/09/%E7%BE%A4%E6%99%96%20Active%20Backup%20for%20Business%20%E6%81%A2%E5%A4%8D%20Windows%20%E8%AE%BE%E5%A4%87/2.avif)
    
3. 第一次创建时，提示需要下载并安装 Windows ADK，这里点击下载
    
    ![](https://assets.flinty.moe/blog/posts/2022/09/%E7%BE%A4%E6%99%96%20Active%20Backup%20for%20Business%20%E6%81%A2%E5%A4%8D%20Windows%20%E8%AE%BE%E5%A4%87/3.avif)
    
4. Windows ADK 的安装程序会自动打开，一路默认下一步。到这一步的时候，只需要勾选“部署工具”和“Windows 预安装环境 (Windows PE)”
    
    ![](https://assets.flinty.moe/blog/posts/2022/09/%E7%BE%A4%E6%99%96%20Active%20Backup%20for%20Business%20%E6%81%A2%E5%A4%8D%20Windows%20%E8%AE%BE%E5%A4%87/4.avif)
    
5. 选择 U 盘，点击创建
    
    ![](https://assets.flinty.moe/blog/posts/2022/09/%E7%BE%A4%E6%99%96%20Active%20Backup%20for%20Business%20%E6%81%A2%E5%A4%8D%20Windows%20%E8%AE%BE%E5%A4%87/5.avif)
    
6. 等他完成就可以拔掉U盘了
    
    ![](https://assets.flinty.moe/blog/posts/2022/09/%E7%BE%A4%E6%99%96%20Active%20Backup%20for%20Business%20%E6%81%A2%E5%A4%8D%20Windows%20%E8%AE%BE%E5%A4%87/6.avif)
    

### 恢复系统

1. 用U盘启动，会进到这里
    
    ![](https://assets.flinty.moe/blog/posts/2022/09/%E7%BE%A4%E6%99%96%20Active%20Backup%20for%20Business%20%E6%81%A2%E5%A4%8D%20Windows%20%E8%AE%BE%E5%A4%87/7.avif)
    
2. 连接 NAS，地址公网和内网都可以
    
    ![](https://assets.flinty.moe/blog/posts/2022/09/%E7%BE%A4%E6%99%96%20Active%20Backup%20for%20Business%20%E6%81%A2%E5%A4%8D%20Windows%20%E8%AE%BE%E5%A4%87/8.avif)
    
3. 选择备份
    
    ![](https://assets.flinty.moe/blog/posts/2022/09/%E7%BE%A4%E6%99%96%20Active%20Backup%20for%20Business%20%E6%81%A2%E5%A4%8D%20Windows%20%E8%AE%BE%E5%A4%87/9.avif)
    
4. 这里的话，如果你的电脑只有一个盘，或者你能保证要恢复的硬盘是第一个硬盘，那就根据备份类型选前两个。
    
    ![](https://assets.flinty.moe/blog/posts/2022/09/%E7%BE%A4%E6%99%96%20Active%20Backup%20for%20Business%20%E6%81%A2%E5%A4%8D%20Windows%20%E8%AE%BE%E5%A4%87/10.avif)
    
    否则他会默认恢复到第一个空闲的硬盘，如果空间不够大会报错。
    
    ![](https://assets.flinty.moe/blog/posts/2022/09/%E7%BE%A4%E6%99%96%20Active%20Backup%20for%20Business%20%E6%81%A2%E5%A4%8D%20Windows%20%E8%AE%BE%E5%A4%87/11.avif)
    
    所以后面的步骤是选择第三个
    
5. 这里直接点 自定义硬盘映射
    
    ![](https://assets.flinty.moe/blog/posts/2022/09/%E7%BE%A4%E6%99%96%20Active%20Backup%20for%20Business%20%E6%81%A2%E5%A4%8D%20Windows%20%E8%AE%BE%E5%A4%87/12.avif)
    
6. 这里右键需要恢复的硬盘，依次恢复所有分区。推荐把C盘放到最后一个，这样以后如果要修改C盘空间会比较方便。
    
    ![](https://assets.flinty.moe/blog/posts/2022/09/%E7%BE%A4%E6%99%96%20Active%20Backup%20for%20Business%20%E6%81%A2%E5%A4%8D%20Windows%20%E8%AE%BE%E5%A4%87/13.avif)
    
7. 都分完就是这样，如果新硬盘比原来的大，会多出一部分空间。这一部分可以进系统后加到C盘里。
    
    ![](https://assets.flinty.moe/blog/posts/2022/09/%E7%BE%A4%E6%99%96%20Active%20Backup%20for%20Business%20%E6%81%A2%E5%A4%8D%20Windows%20%E8%AE%BE%E5%A4%87/14.avif)
    
8. 点击确定后，可以看到这边备份的分区都分好了，点击下一步
    
    ![](https://assets.flinty.moe/blog/posts/2022/09/%E7%BE%A4%E6%99%96%20Active%20Backup%20for%20Business%20%E6%81%A2%E5%A4%8D%20Windows%20%E8%AE%BE%E5%A4%87/15.avif)
    
9. 确认无误后，就可以点击确定开始恢复了。
    
    ![](https://assets.flinty.moe/blog/posts/2022/09/%E7%BE%A4%E6%99%96%20Active%20Backup%20for%20Business%20%E6%81%A2%E5%A4%8D%20Windows%20%E8%AE%BE%E5%A4%87/16.avif)
    
10. 恢复时间不是很准，他是假设整个分区都有内容估算的时间。如果你原来C盘只用了一半，那实际时间大概就是它估算的一半。
    
    ![](https://assets.flinty.moe/blog/posts/2022/09/%E7%BE%A4%E6%99%96%20Active%20Backup%20for%20Business%20%E6%81%A2%E5%A4%8D%20Windows%20%E8%AE%BE%E5%A4%87/17.avif)
    
11. 恢复成功后，重启就可以进系统了
    
    ![](https://assets.flinty.moe/blog/posts/2022/09/%E7%BE%A4%E6%99%96%20Active%20Backup%20for%20Business%20%E6%81%A2%E5%A4%8D%20Windows%20%E8%AE%BE%E5%A4%87/18.avif)