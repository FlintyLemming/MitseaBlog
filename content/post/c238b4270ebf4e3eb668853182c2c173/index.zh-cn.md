+++
author = "FlintyLemming"
title = "TrueNAS Core 使用 Jail 安装 qBittorrent"
slug = "c238b4270ebf4e3eb668853182c2c173"
date = "2023-04-04"
description = ""
categories = ["HomeLab"]
tags = ["FreeBSD", "TrueNAS Core", "qBittorrent"]
image = "https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/04/TrueNAS%20Core%20%E4%BD%BF%E7%94%A8%20Jail%20%E5%AE%89%E8%A3%85%20qBittorrent/marek-piwnicki-i5AF9bW10p0-unsplash.avif"
+++

## 开头

TrueNAS Core 由于使用 FreeBSD，zfs 的性能似乎比在 Linux 上好不少。不过也导致了没有 Docker 和 k3s 的问题，安装程序多有不便。

虽然 Core 提供了一个商店，但是在安装时由于国内网络原因，我即便设置了系统的 http 代理安装也会失败。不过 FreeBSD 有个 Jail 的功能，倒是可以拿来安装 qBittorrent。Jail 我觉得倒不像 Docker，反倒像 lxc，下面的步骤中，需要先创建一个 Jail，然后像在空白系统里一样下载并启动 qBittorrent。

## 步骤

### 准备安装所需文件夹

1. 创建一个存放 qBittorrent 配置文件的文件夹，和下载文件夹。比如我用 AppData 存放配置文件，Downloads 存放下载文件。

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/04/TrueNAS%20Core%20%E4%BD%BF%E7%94%A8%20Jail%20%E5%AE%89%E8%A3%85%20qBittorrent/Untitled.avif)

2. 记下两个文件夹的路径，TrueNAS Core 的路径是 /mnt/<Pool名>/<dataset名>/……。我的话，配置文件的路径是 /mnt/pool0/AppData/qbittorrent，下载路径是 /mnt/pool0/Downloads
3. 配置 ACL，这里不需要给 Jail 额外配置什么权限，给你自己 smb 访问的账户配好权限就行

### 创建和配置 Jail

1. 创建 Jail，点击高级设置

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/04/TrueNAS%20Core%20%E4%BD%BF%E7%94%A8%20Jail%20%E5%AE%89%E8%A3%85%20qBittorrent/Untitled%201.avif)

2. Basic Properties 这里有几个地方修改一下：① 填写一个名称；② Release 选择一个最新版；③ 勾选 DHCP，此时下方的 VNET 和 Berkeley Packet Filter 会被自动勾选；④ 选择联网的网口

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/04/TrueNAS%20Core%20%E4%BD%BF%E7%94%A8%20Jail%20%E5%AE%89%E8%A3%85%20qBittorrent/Untitled%202.avif)

    其他地方不用动，点击 Next

3. Jail Properties 一个都不需要动，直接 Next

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/04/TrueNAS%20Core%20%E4%BD%BF%E7%94%A8%20Jail%20%E5%AE%89%E8%A3%85%20qBittorrent/Untitled%203.avif)

4. Network Properties 这里都不需要动，直接 Next

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/04/TrueNAS%20Core%20%E4%BD%BF%E7%94%A8%20Jail%20%E5%AE%89%E8%A3%85%20qBittorrent/Untitled%204.avif)

5. Custom Properties 这里都不需要动，直接 Save

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/04/TrueNAS%20Core%20%E4%BD%BF%E7%94%A8%20Jail%20%E5%AE%89%E8%A3%85%20qBittorrent/Untitled%205.avif)

6. 保存后，回到 Jails 管理画面，展开刚才创建的 Jail，启动它。然后点击 Shell 进入 Jail，这里需要先把 qBittorrent 的 config 和下载文件夹创建好

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/04/TrueNAS%20Core%20%E4%BD%BF%E7%94%A8%20Jail%20%E5%AE%89%E8%A3%85%20qBittorrent/Untitled%206.avif)

    创建文件夹

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/04/TrueNAS%20Core%20%E4%BD%BF%E7%94%A8%20Jail%20%E5%AE%89%E8%A3%85%20qBittorrent/Untitled%207.avif)

7. 关闭 Shell，再把 Jail 也停止。停止后，点击 Mount Points

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/04/TrueNAS%20Core%20%E4%BD%BF%E7%94%A8%20Jail%20%E5%AE%89%E8%A3%85%20qBittorrent/Untitled%208.avif)

8. 新建文件夹映射，这里上面是 NAS 的文件目录，下面是 Jail 内的。这里映射的是配置文件夹的，下载文件夹同理

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/04/TrueNAS%20Core%20%E4%BD%BF%E7%94%A8%20Jail%20%E5%AE%89%E8%A3%85%20qBittorrent/Untitled%209.avif)

    配置好后大概是这样

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/04/TrueNAS%20Core%20%E4%BD%BF%E7%94%A8%20Jail%20%E5%AE%89%E8%A3%85%20qBittorrent/Untitled%2010.avif)

### 安装并启动 qBittorrent

1. 启动刚才的 Jail，并进入 Shell，更新软件包目录

    ```bash
    pkg update && pkg upgrade
    ```

    如果网络原因卡住或者报错，可以参考这篇文章修改软件源[FreeBSD 更换中科大软件包源](https://blog.mitsea.com/c877a212f3e347d8b414c2c3afe4e001/)

2. 安装 qBittorrent

    ```bash
    pkg install qbittorrent-nox
    ```

3. 配置服务

    ```bash
    sysrc qbittorrent_enable=YES
    ```

4. 配置 qBittorrent 使用的配置文件夹

    ```bash
    sysrc qbittorrent_conf_dir=/mnt/config
    ```

5. 设置读写权限，我这边偷懒了，下载文件也没啥重要的，就直接 777 了

    ```bash
    chmod -R 777 /mnt/config
    chmod -R 777 /mnt/downloads
    ```

6. 启动 qBittorrent

    ```bash
    service qbittorrent start
    ```

### 访问 qBittorrent

前面说了，Jail 类似与 lxc，或者是虚拟机，所以他默认是桥接本机网络，Jail 获得了一个内网 IP。在管理页面可以看到

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/04/TrueNAS%20Core%20%E4%BD%BF%E7%94%A8%20Jail%20%E5%AE%89%E8%A3%85%20qBittorrent/Untitled%2011.avif)

我这边就是打开 [http://192.168.2.121:8080/](http://192.168.2.121:8080/) 即可

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/04/TrueNAS%20Core%20%E4%BD%BF%E7%94%A8%20Jail%20%E5%AE%89%E8%A3%85%20qBittorrent/Untitled%2012.avif)

默认用户是 admin，默认密码是 adminadmin

> Photo by [Marek Piwnicki](https://unsplash.com/@marekpiwnicki?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
