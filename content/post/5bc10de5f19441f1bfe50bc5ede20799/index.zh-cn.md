+++
author = "FlintyLemming"
title = "TrueNAS 定时同步配置"
slug = "5bc10de5f19441f1bfe50bc5ede20799"
date = "2023-12-31T01:00:00+08:00"
description = ""
categories = ["HomeLab"]
tags = ["TrueNAS", "rsync"]
image = "https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2023/12/TrueNAS%20%E5%AE%9A%E6%97%B6%E5%90%8C%E6%AD%A5%E9%85%8D%E7%BD%AE/rick-rothenberg-Ih4tXZrOkMI-unsplash.avif"
+++

TrueNAS 的同步功能我挺喜欢的，因为我文件比较多，可管理的定时非实时同步对我来说很实用，也比实时同步更可靠。

但是 TrueNAS 的同步配置起来跟其他系统或者软件不太一样，有点扭曲，这里帮助需要使用的人快速上手。

主要介绍 Rsync 和 WebDAV，前者介绍的是 Module 方式的同步，因为我看网上说的都是走 SSH 传输。我自己更喜欢 Module 这种用户名和密码方式的验证，简单方便。

## Rsync

### 基本配置

首先对基本配置里的几个点做补充

![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2023/12/TrueNAS%20%E5%AE%9A%E6%97%B6%E5%90%8C%E6%AD%A5%E9%85%8D%E7%BD%AE/Untitled.avif)

1. 若要同步文件夹里的内容，这里要加一个斜杠，默认不会加
2. 用户这里建议选择和使用 Samba 相同的用户（一般就是除了 root 之外你自己手动创建的那个用户）
3. Push 就是同步到对方的设备上，Pull 就是同步到本机
4. 用户名加在地址的前面
5. 模块名可以自己用下面命令拉个 list，群晖的话默认就是共享文件夹的列表

    ```bash
    rsync rsync://[服务器地址]/
    ```

### 创建密码文件

密码似乎不能直接写在环境变量里，我这里是创建了一个密码文件

1. 在上面的基本设置里，确认你的账户名，然后要创建一个只能被该用户访问的密码文件。所以首先在这个用户所有权的目录里创建密码文件。第一步是找到账户目录。打开命令行执行下面的命令即可

    ```bash
    su - username
    ```

2. 创建密码文件。客户端的密码文件只需要有密码的明文，不需要有用户名和冒号以及其他内容

    ```bash
    nano rsync.secrets
    ```

3. 设置该文件不允许其他用户访问。否则就会报 `ERROR: password file must not be other-accessible` 的错误。由于是在所有者为 username 的文件夹里创建的文件，所以执行该操作后，这个文件就只允许 username 访问

    ```bash
    chmod 600 rsync.secrets
    ```

### 具体参数配置

这里就是比较扭曲的地方了，他这个同步的 WebUI 功能做的不全，很多常用的功能还要自己加参数

![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2023/12/TrueNAS%20%E5%AE%9A%E6%97%B6%E5%90%8C%E6%AD%A5%E9%85%8D%E7%BD%AE/Untitled%201.avif)

首先要说明的是这里的参数格式为 --[参数名]=”[参数内容]”。然后说几个常用参数，其他的可以自己查。

1. password-file 这里内容填刚才创建的密码文件
2. log-file 建议用，因为如果报错了 TrueNAS 不会显示错误原因
3. exclude 用来排除文件夹，如果你开了快照，建议排除 .zfs，不然就会把快照内容也同步到对方机器中

### 补充-群晖的 Rsync 服务器

原生 Linux 中，用户的登陆密码、Samba 密码、Rsync 服务端密码其实是三个东西。群晖的 Rsync 密码就需要我们主动设置。设置位置在这里，密码一定要设置，只选择一个用户是不会继承登陆密码的。

![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2023/12/TrueNAS%20%E5%AE%9A%E6%97%B6%E5%90%8C%E6%AD%A5%E9%85%8D%E7%BD%AE/Untitled%202.avif)

## WebDAV

这个相比于 Rsync 就没有那些弯弯绕了。先在云凭据里创建凭据

![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2023/12/TrueNAS%20%E5%AE%9A%E6%97%B6%E5%90%8C%E6%AD%A5%E9%85%8D%E7%BD%AE/Untitled%203.avif)

然后在云同步里设置同步就行了

![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2023/12/TrueNAS%20%E5%AE%9A%E6%97%B6%E5%90%8C%E6%AD%A5%E9%85%8D%E7%BD%AE/Untitled%204.avif)

> Photo by [Rick Rothenberg](https://unsplash.com/@rick_rothenberg?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/a-close-up-of-a-bunch-of-green-plants-Ih4tXZrOkMI?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)
  