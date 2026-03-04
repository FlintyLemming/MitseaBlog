+++
author = "FlintyLemming"
title = "在 btrfs 文件系统上离线安装 AOSC OS"
slug = "311779ab64688073b953fcff47ae64c3"
date = "2026-03-04"
description = "参考 Fedora 的分区设置"
categories = ["HomeLab"]
tags = ["AOSC"]
image = "https://assets.mitsea.cn/blog/posts/2026/03/%E5%9C%A8%20btrfs%20%E6%96%87%E4%BB%B6%E7%B3%BB%E7%BB%9F%E4%B8%8A%E7%A6%BB%E7%BA%BF%E5%AE%89%E8%A3%85%20AOSC%20OS/frode-myklebust-UbDdynMmcX0-unsplash.jpg"
+++

## 准备操作

进入命令行模式安装系统

![](https://assets.mitsea.cn/blog/posts/2026/03/%E5%9C%A8%20btrfs%20%E6%96%87%E4%BB%B6%E7%B3%BB%E7%BB%9F%E4%B8%8A%E7%A6%BB%E7%BA%BF%E5%AE%89%E8%A3%85%20AOSC%20OS/%E5%9B%BE%E7%89%87.png)

由于下面的操作比较多，为了方便复制，可以考虑使用 ssh 远程连接

设置 root 密码

```bash
passwd
```

修改 ssh 配置以启用 ssh 远程登陆

```bash
nano /etc/ssh/sshd_config
```

将`PremitRootLogin` 前的注释删掉并改成 `yes`

重启 ssh 服务

```bash
systemctl restart ssh
```

## 格式化硬盘与分区

假设硬盘是 `/dev/sda`

### 清除硬盘内容

1. 擦掉所有文件系统签名
    
    ```bash
    wipefs -a /dev/sda*
    ```
    
2. 删除 GPT/MBR 分区表
    
    ```bash
    sgdisk --zap-all /dev/sda
    ```
    

### 分区与格式化

1.  用 `fdisk` 重建 GPT
    
    ```bash
    fdisk /dev/sda
    ```
    
2. 在 fdisk 里依次输入，每行一个命令（期间如果提示分区包含一个 xx 签名，选择 y 移除签名）：
    
    `g` （新建 GPT）
    
    `n` （新建分区 1：EFI）
    
    回车（默认分区号 1）
    
    回车（默认起始）
    
    输入：`+600M` 
    
    `t` （改类型）
    
    输入：`1` 选择 EFI System（通常会让你选类型，直接选 EFI/1）
    
    `n` （新建分区 2：boot 分区）
    
    回车（默认分区号 2）
    
    回车（默认起始）
    
    输入：`+1.4G` （如果需要多内核可以调大）
    
    `n` （新建分区 3：btrfs 系统分区）
    
    - 回车（默认分区号 3）
    - 回车（默认起始）
    - 回车（用剩余全部空间）
    
    `w` （写入并退出）
    
3. 写完后检查
    
    ```bash
    lsblk /dev/sda
    ```
    
    你应该看到 `sda1` 和 `sda2` 出现。
    
4. 格式化
    
    EFI 分区（FAT32）
    
    ```bash
    mkfs.fat -F32 /dev/sda1
    ```
    
    boot 分区（ext4）
    
    ```bash
    mkfs.ext4 -L BOOT /dev/sda2
    ```
    
    btrfs 分区
    
    ```bash
    mkfs.btrfs -f -L LINUX_SYSTEM /dev/sda3
    ```
    

### 创建 btrfs 子卷并挂载（@ 和 @home）

1. 先临时挂载顶层
    
    ```bash
    mkdir -p /mnt/btrfs-top
    mount /dev/sda3 /mnt/btrfs-top
    ```
    
2. 创建子卷
    
    ```bash
    btrfs subvolume create /mnt/btrfs-top/@
    btrfs subvolume create /mnt/btrfs-top/@home
    ```
    
3. 卸载顶层
    
    ```bash
    umount /mnt/btrfs-top
    ```
    
4. 挂载安装用的根目录和系统分区
    
    ```bash
    # 挂载带透明压缩的 btrfs 根目录到 /mnt
    mount -o subvol=@,compress=zstd:3 /dev/sda3 /mnt
    
    # 挂载独立的 ext4 boot 分区
    mkdir -p /mnt/boot
    mount /dev/sda2 /mnt/boot
    
    # 挂载 EFI 分区
    mkdir -p /mnt/boot/efi
    mount /dev/sda1 /mnt/boot/efi
    
    # 挂载带透明压缩的 btrfs home 目录
    mkdir -p /mnt/home
    mount -o subvol=@home,compress=zstd:3 /dev/sda3 /mnt/home
    ```
    

## 安装系统

### 复制系统文件

1. 确认一下你要用的源目录是存在的：
    
    ```
    ls /run/livekit/sysroots/desktop | head
    ```
    
    如果能列出 `/etc` `/lib` 之类，确认你的 livekit 确实是有离线包，我们就开始使用 rsync 复制
    
2. 执行 rsync 命令复制
    
    ```jsx
    rsync -aAXHv \
      --exclude={"/dev/*","/proc/*","/sys/*","/tmp/*","/run/*","/mnt/*","/media/*","/lost+found"} \
      /run/livekit/sysroots/desktop/ \
      /mnt/
    ```
    

### 生成 fstab

1. 先获取 UUID
    
    ```
    blkid
    ```
    
    记住：
    
    - sda3 的 UUID（btrfs 系统分区）
    - sda2 的 UUID（ext4 boot 分区）
    - sda1 的 UUID（FAT32 EFI 分区）
2. 编辑 fstab：
    
    ```
    nano /mnt/etc/fstab
    ```
    
    写入（用你实际 UUID 替换）：
    
    ```
    UUID=<sda3的UUID>  /          btrfs  subvol=@,compress=zstd:3      0 0
    UUID=<sda2的UUID>  /boot      ext4   defaults                      0 2
    UUID=<sda3的UUID>  /home      btrfs  subvol=@home,compress=zstd:3  0 0
    UUID=<sda1的UUID>  /boot/efi  vfat   defaults                      0 1
    ```
    
    保存退出
    

### 准备 chroot

1. 挂载系统目录
    
    ```
    mount --bind /dev /mnt/dev
    mount --bind /proc /mnt/proc
    mount --bind /sys /mnt/sys
    mount --bind /sys/firmware/efi/efivars /mnt/sys/firmware/efi/efivars
    mount --bind /run /mnt/run
    ```
    
2. 然后进入系统：
    
    ```
    chroot /mnt /bin/bash
    ```
    
    如果成功，你的提示符会变化，会被重新定位到 `/`。期间如果提示 “tty: ttyname error: No such device” 错误是正常的。
    

### 杂项

1. 生成 machine-id
    
    ```
    systemd-machine-id-setup
    ```
    
2. 重建 initramfs
    
    ```
    dracut -f
    ```
    

## 安装 GRUB（UEFI）

1. 确认 EFI 挂载：
    
    ```
    ls /boot/efi
    ```
    
    没返回是正常的，有这个文件夹就行
    
2. 安装
    
    ```
    grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=AOSC
    ```
    
3. 然后生成配置：
    
    ```
    grub-mkconfig -o /boot/grub/grub.cfg
    ```
    

## 后处理

### 设置 root 密码

```
passwd
```

### 退出并重启

```
exit
umount -R /mnt
reboot
```

安装完成

![](https://assets.mitsea.cn/blog/posts/2026/03/%E5%9C%A8%20btrfs%20%E6%96%87%E4%BB%B6%E7%B3%BB%E7%BB%9F%E4%B8%8A%E7%A6%BB%E7%BA%BF%E5%AE%89%E8%A3%85%20AOSC%20OS/%E5%9B%BE%E7%89%87%201.png)

> Photo by [Frode Myklebust](https://unsplash.com/@famyklebust?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/photos/hills-and-ocean-under-a-pastel-sky-UbDdynMmcX0?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
      