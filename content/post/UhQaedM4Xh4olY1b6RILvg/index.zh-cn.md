+++
author = "FlintyLemming"
title = "使用 restic 进行多版本备份"
slug = "UhQaedM4Xh4olY1b6RILvg"
date = "2025-04-22"
description = ""
categories = ["HomeLab"]
tags = ["备份", "restic"]
image = "https://hf-image.mitsea.com:8840/blog/posts/2025/04/%E4%BD%BF%E7%94%A8%20restic%20%E8%BF%9B%E8%A1%8C%E5%A4%9A%E7%89%88%E6%9C%AC%E5%A4%87%E4%BB%BD/steve-gribble-tuRraTuflBA-unsplash.avif"
+++

## 背景

现有备份服务器 A，业务服务器 B、C。A 仅有最基本的 lvm + xfs 存储和 ssh 连接。为了满足多版本备份，选择使用 [restic](https://restic.net/) 备份方案。

restic 的优势主要有以下几点：

1. 备份目的地不需要安装额外软件，没有环境要求，多种连接方式

2. 去重、多版本、加密、快速

3. 开源、多平台支持、使用简单

下面的步骤以服务器 B 备份到 A 为例，C 同理。

## 环境准备

### 安装 restic

备份服务器 A 不需要安装任何程序，在业务服务器 B 和 C 上安装 restic 即可，直接在 apt 源安装即可

```bash
sudo apt install restic
```

### 配置 ssh key 免密登陆

创建密钥对和配置过程不赘述，需要注意一点的是，由于备份时使用 sudo 备份，（因为有的文件是由 docker 作为 root 创建出来的），所以你的密钥需要配置在 root 用户 home 下的 .ssh 文件夹里，而不是普通用户的 home 里。

## 创建备份仓库和初次备份

1. 在 B 服务器上执行命令，在 A 服务器上创建备份 repo
    
    ```bash
    sudo restic init --repo sftp:aiteam@ServerA_IP:/export/aiteam/restic-repo/r750xa
    ```
    
    执行后会让你输入一个密码，设置好后要记住这个密码
    
2. 在本机创建一个密码文件保存刚才设置的密码，为后续自动化备份做准备
    
    ```bash
    sudo mkdir -p /etc/restic
    sudo bash -c 'echo "你的仓库密码" > /etc/restic/password' # 把 "你的仓库密码" 替换成真实的密码
    sudo chmod 600 /etc/restic/password
    ```
    
3. 尝试初次备份，验证配置
    
    ```bash
    sudo restic --repo sftp:aiteam@ServerA_IP:/export/aiteam/restic-repo/r750xa \
           --password-file /etc/restic/password \
           backup /mnt/extend/appdata /mnt/extend/Projects
    ```
    
    ![](https://hf-image.mitsea.com:8840/blog/posts/2025/04/%E4%BD%BF%E7%94%A8%20restic%20%E8%BF%9B%E8%A1%8C%E5%A4%9A%E7%89%88%E6%9C%AC%E5%A4%87%E4%BB%BD/image.avif)
    

## 仓库验证和快照管理

### **检查仓库完整性**

```bash
sudo restic --repo sftp:aiteam@ServerA_IP:/export/aiteam/restic-repo/r750xa \
       --password-file /etc/restic/password \
       check
```

### 列出所有快照

```bash
sudo restic --repo sftp:aiteam@ServerA_IP:/export/aiteam/restic-repo/r750xa \
       --password-file /etc/restic/password \
       snapshots
```

### 管理旧快照

由于备份没有长期保存的需求，所以我这边选用最简单的保留近 7 天快照

```bash
sudo restic --repo sftp:aiteam@ServerA_IP:/export/aiteam/restic-repo/r750xa \
       --password-file /etc/restic/password \
       forget --keep-daily 7 --prune
```

## 自动化

使用 crontab 自动化即可，每天执行一次备份、删除旧快照，即可保证始终有近 7 天的版本。可以使用 Restic Browser 便捷的查看历史版本，大概就是这样的效果

![](https://hf-image.mitsea.com:8840/blog/posts/2025/04/%E4%BD%BF%E7%94%A8%20restic%20%E8%BF%9B%E8%A1%8C%E5%A4%9A%E7%89%88%E6%9C%AC%E5%A4%87%E4%BB%BD/iShot_2025-04-22_21.16.10.avif)

## 关于 Docker

官方有提供 Docker，但大概就是一个执行 restic 二进制的容器，如果要通过 SSH 连接配置 key 的话，不是很方便，可以考虑使用 `mazzolino/restic` 这个镜像，在镜像内操作

> Photo by [Steve Gribble](https://unsplash.com/@steve_g_?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/a-mountain-under-a-starry-night-sky-tuRraTuflBA?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)
      