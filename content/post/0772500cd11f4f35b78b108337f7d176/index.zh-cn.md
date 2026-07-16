+++
author = "FlintyLemming"
title = "龙芯 3A6000 手搓 NAS 记录与平台体验"
slug = "0772500cd11f4f35b78b108337f7d176"
date = "2023-11-05"
description = "体验不错，未来可期"
categories = ["HomeLab", "Linux"]
tags = ["Loongson", "NAS"]
image = "https://assets.flinty.moe/blog/posts/2023/10/%E9%BE%99%E8%8A%AF%203A6000%20%E6%89%8B%E6%90%93%20NAS%20%E8%AE%B0%E5%BD%95%E4%B8%8E%E5%B9%B3%E5%8F%B0%E4%BD%93%E9%AA%8C/eberhard-grossgasteiger-W7l2qAUKWcs-unsplash.avif"
+++

最近到收了个龙芯 3A6000 的平台，简单测了下，首先由于是自主架构，所以 Windows 肯定是不行。然后内置 GPU 只能亮机，试了 Arch、UOS、AOSC 在安装或者启动时都不同程度遇到显示问题，桌面基本上是不可用。不过考虑到 loongarch64 Linux 内核已经并入主线，所以硬件支持和基本 Linux 软件还是没问题，拿来做 NAS 似乎还不错。

![](https://assets.flinty.moe/blog/posts/2023/10/%E9%BE%99%E8%8A%AF%203A6000%20%E6%89%8B%E6%90%93%20NAS%20%E8%AE%B0%E5%BD%95%E4%B8%8E%E5%B9%B3%E5%8F%B0%E4%BD%93%E9%AA%8C/IMG_4370.avif)

由于我已经有一个群晖 DS1621+，所以这台机器的定位就是取代我当前使用 unraid 作为 HyperBackup 备份 NAS 的角色。梳理了一下他主要承担如下几个作用：smb 共享、rsync 服务端做备份、qBittorrent pt下载和保种。

普通 Linux 做简单 NAS 很简单，按照 ChatGPT 的指导操作就行，我主要记录下龙芯平台目前遇到的坑。介绍前先放 NAS 场景的部分测试。

## 简单测试

### 下述软件包运行情况

![](https://assets.flinty.moe/blog/posts/2023/10/%E9%BE%99%E8%8A%AF%203A6000%20%E6%89%8B%E6%90%93%20NAS%20%E8%AE%B0%E5%BD%95%E4%B8%8E%E5%B9%B3%E5%8F%B0%E4%BD%93%E9%AA%8C/Untitled.avif)

### qBittorrent 高强度下载测试

![](https://assets.flinty.moe/blog/posts/2023/10/%E9%BE%99%E8%8A%AF%203A6000%20%E6%89%8B%E6%90%93%20NAS%20%E8%AE%B0%E5%BD%95%E4%B8%8E%E5%B9%B3%E5%8F%B0%E4%BD%93%E9%AA%8C/Untitled%201.avif)

下载速度 259MB/s 连接用户 575 完全不卡，性能完全够用。这个速度我在 AMD V1500B 上用 Docker 跑 WebUI 就完全打不开了。

### 网络性能测试

![](https://assets.flinty.moe/blog/posts/2023/10/%E9%BE%99%E8%8A%AF%203A6000%20%E6%89%8B%E6%90%93%20NAS%20%E8%AE%B0%E5%BD%95%E4%B8%8E%E5%B9%B3%E5%8F%B0%E4%BD%93%E9%AA%8C/Untitled%202.avif)

1500 MTU 单线程跑满 10Gbps

## 选择发行版

龙芯的发行版分为新世界和旧世界，旧世界我直接排除了，选择 Linux 6.x 内核的新世界发行版。

[旧世界与新世界 | 咱龙了吗？](https://areweloongyet.com/docs/old-and-new-worlds/)

新世界我看了下主要有几个发行版：Arch Linux、Debian、AOSC，其他我也试了几个，我主要说一下这三点的体验。我最终选的是 pve。

### Arch Linux

[下载地址](https://mirrors.wsyu.edu.cn/loongarch/archlinux/iso/)

**pros**

软件包似乎是最全的 Linux 发行版，常用工具都是开箱即用

维护比较勤快，有专人负责，有论坛社区

**cons**

万兆网卡 lspci 能识别到但是 ip link 里看不到，直接就用不了。这个问题先发了论坛[帖子](https://bbs.loongarch.org/d/313-intel-x710)等修

如果用 GUI 的话，正如开头所说的，内置核显不好使，建议加一块（推荐为）北极星架构的 AMD 显卡

### Debian

Debian 据说官方已经支持 loongarch64，但我官网没找到 ISO 包，所以我是直接下的 pve，四舍五入也是 Debian。[下载地址](https://mirrors.apqa.cn/proxmox/isos/) [发布地址](https://foxi.buduanwang.vip/virtualization/pve/2924.html/)

**pros**

软件包也很全，常用工具我看也都有

虚拟机用起来没什么问题，要折腾的话可以考虑先装这个，然后把其他发行版都体验一圈

PCIe 硬件使用都正常

**cons**

zfs 不能用

ISO 安装包的 EFI 引导文件似乎命名不规范，导致 3A6000 没法直接从安装盘启动，需要在 BIOS 手动创建启动项或者在 UEFI Shell 里手动执行 efi 文件启动

### AOSC

[发行地址](https://aosc.io)

**pros**

社区活跃，开发者积极

PCIe 硬件用起来也都正常

**cons**

官方软件源里的软件包太少，而且有的依赖包版本有点老，导致即便从别的发行版里偷二进制文件来用都不一定好使

如果用 GUI 的话也是跟 Arch 一样，否则开机显示桌面直接黑屏

## scrutiny 监控硬盘 SMART

因为我已经安装过 Web 端，所以我只需要在这台机器上定时跑 collector 即可。

安装 `golang-go` 和 `make` 后，从 [GitHub 仓库](https://github.com/AnalogJ/scrutiny/releases)下载源码直接 `make binary-collector` 就可以把二进制文件 `scrutiny-collector-metrics` 编译出来，之后编辑计划任务定时跑就行。

在 [https://disks.mitsea.com](https://disks.mitsea.com) 可以看到我的所有硬盘

## mdadm 创建阵列并挂载

由于我有7个相同容量的 HDD，所以我可以直接用 mdadm 创建一个 raid5 阵列。mdadm 软件包里有，直接 apt install 即可。下面简单介绍下步骤，所有分区操作不详细说明，直接用 cfdisk 很简单。

1. 清理硬盘，删除硬盘上所有分区，然后每个磁盘创建一个普通 ext4 分区
2. 使用 mdadm 创建 raid5 阵列，命令 be like

    ```bash
    sudo mdadm --create --verbose /dev/md0 --level=5 --raid-devices=3 /dev/sda1 /dev/sdb1 /dev/sdc1
    ```

3. 等待阵列创建完毕，一定要等待创建完再进行下面的步骤，不然会卡 I/O。可以通过下面的命令查看进度

    ```bash
    cat /proc/mdstat
    sudo mdadm --detail /dev/md0
    ```

4. 格式化

    ```bash
    sudo mkfs.ext4 /dev/md0
    ```

5. 挂载

    ```bash
    sudo mkdir /mnt/raid5
    sudo mount /dev/md0 /mnt/raid5
    ```

## 配置 Samba

samba 也是可以直接安装，软件包里有

1. 备份原始配置文件：

    ```bash
    sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.backup
    ```

2. 编辑 Samba 的配置文件来添加分享：

    ```bash
    sudo nano /etc/samba/smb.conf
    ```

    在文件的底部添加：

    ```config
    [NAS]
    path = /mnt/raid5
    writeable = yes
    browseable = yes
    create mask = 0777
    directory mask = 0777
    public = no
    ```

3. 创建 samba 用户，如果是 root 就使用 `adduser your_username` 创建一个新用户后再执行

    ```bash
    sudo smbpasswd -a your_username
    ```

4. 如果用户的用户组权限不够的话，可以改一下文件夹权限

    ```bash
    sudo chown your_username:your_username /path/to/your/directory
    sudo chmod 0700 /path/to/your/directory
    ```

5. 重启 Samba 服务：

    ```bash
    sudo service smbd restart
    ```

## 配置 Rsync 服务端

rsync 软件包里也有，直接安装就行

1. 使用 `sudo nano /etc/rsyncd.conf` 修改配置文件，内容 be like

    ```config
    uid = <有权限的用户的uid>
    gid = <有权限的用户的gid>
    use chroot = yes
    max connections = 10
    strict modes = yes
    log file = /var/log/rsync.log
    timeout = 300
    
    [HyperBackup]
    path = /mnt/raid5/HyperBackup
    comment = Backup folder
    read only = no
    list = yes
    auth users = flintylemming
    secrets file = /etc/rsyncd.secrets
    hosts allow = YOUR_ALLOWED_IP_RANGE_OR_ADDRESS
    ```

2. 使用 `sudo nano /etc/rsyncd.secrets` 创建密码文件，内容 be like

    ```plain text
    flintylemming:xxxx
    ```

3. 启动 rsync

    ```bash
    sudo rsync --daemon
    ```

## 安装 qBittorrent

软件包里也有，直接安装就行。不要 GUI 的话，就安装 qbittorrent-nox

这个没什么好配置的，直接启动后在 WebUI 里配置就行。新建一个 screen 或者创建一个 systemctl 服务直接运行 qbittorrent-nox 即可。建议运行前先执行 `export LANG=zh_CN.UTF-8` 把当前会话的 shell 语言改成中文，不然下载含有汉字的文件夹资源，汉字都会变成.。

## 简评

从之前听闻 LoongArch 合并入 Linux 5.19 主线后，到我实际上手 3A6000 以一个垃圾佬的角度测试设备的情况来说，我认为说龙芯平台对于主流硬件支持已经比较好了，驱动基本不缺。而且性能也有主流水平。

软件方面 gcc、go、python 等主流语言框架也都支持，维护比较好的发行版软件源对于常用软件也基本做到了开箱即用，少数不能用的自己编译一下也不太麻烦。

还没有体验过 CPU 规模比较大的龙芯产品，但我觉得只要把软件包这块积极维护好还是未来可期的。

> Photo by [eberhard 🖐 grossgasteiger](https://unsplash.com/@eberhardgross?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/W7l2qAUKWcs?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)
