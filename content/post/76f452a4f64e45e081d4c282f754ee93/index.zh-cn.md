+++
author = "FlintyLemming"
title = "NAS 只用后台网页 SSH 本机 Shell"
slug = "76f452a4f64e45e081d4c282f754ee93"
date = "2023-01-23"
description = ""
categories = ["HomeLab"]
tags = ["NAS", "Docker"]
image = "https://img.flinty.moe/blog/posts/2023/01/NAS%20%E5%8F%AA%E7%94%A8%E5%90%8E%E5%8F%B0%E7%BD%91%E9%A1%B5%20SSH%20%E6%9C%AC%E6%9C%BA%20Shell/thai-an-7esp-3K0z28-unsplash.avif"
+++

有的人 NAS 远程可能只用了官方提供的网页穿透或者只映射了后台网页，此时如果有临时访问 NAS Shell 命令行的话就麻烦了，毕竟没有映射 22 SSH 端口。本篇文章介绍下解决方法，并在 QNAP 和 Synology 上实际演示操作。该方法需要用到 Docker。

## 原理

Docker 的子网实际上是从 NAS 自己桥接出来的，你可以理解 NAS 自己是一个路由器，Docker 每个容器都是一个子网设备。那子网设备去连接网关设备，实际上就连接到了 NAS 本身。

## 实际操作演示

### QNAP

1. 安装 Container Station 容器工作站。需要设备至少有 4G 内存，arm64 和 x86_64 都可以。
2. 在 创建 选项卡里搜索 `arangodb/ssh-client` 这个镜像，然后点 安装。这个镜像是 arm64 和 x86_64 都兼容的。
    
    ![](https://img.flinty.moe/blog/posts/2023/01/NAS%20%E5%8F%AA%E7%94%A8%E5%90%8E%E5%8F%B0%E7%BD%91%E9%A1%B5%20SSH%20%E6%9C%AC%E6%9C%BA%20Shell/Untitled.avif)
    
3. 命令 这里一定要改，它原来是 ssh，要改成 /bin/sh。他原来的意思是我们直接在 进入点 那里输入 ssh 参数直接连接，但是在 QNAP WebUI 上不好操作，所以我们先从它的 shell 启动起来。
    
    ![](https://img.flinty.moe/blog/posts/2023/01/NAS%20%E5%8F%AA%E7%94%A8%E5%90%8E%E5%8F%B0%E7%BD%91%E9%A1%B5%20SSH%20%E6%9C%AC%E6%9C%BA%20Shell/Untitled%201.avif)
    
4. 回到 总览，点击启动的容器右侧的 终端机 图标
    
    ![](https://img.flinty.moe/blog/posts/2023/01/NAS%20%E5%8F%AA%E7%94%A8%E5%90%8E%E5%8F%B0%E7%BD%91%E9%A1%B5%20SSH%20%E6%9C%AC%E6%9C%BA%20Shell/Untitled%202.avif)
    
5. 命令你可以直接写 ssh 的命令，不过我这里先进 shell，一样的
    
    ![](https://img.flinty.moe/blog/posts/2023/01/NAS%20%E5%8F%AA%E7%94%A8%E5%90%8E%E5%8F%B0%E7%BD%91%E9%A1%B5%20SSH%20%E6%9C%AC%E6%9C%BA%20Shell/Untitled%203.avif)
    
6. 点击连接后，就会新打开一个标签页，这个无论你是用的 QNAP 自己的穿透，还是自己通过 nginx 反代到公网都是可以打开的。打开就是这个容器内部的 shell。
    
    💡 如果是自己使用 nginx 将 NAS 后台反代到公网，请务必确保已启用 websocket 支持
    
    ![](https://img.flinty.moe/blog/posts/2023/01/NAS%20%E5%8F%AA%E7%94%A8%E5%90%8E%E5%8F%B0%E7%BD%91%E9%A1%B5%20SSH%20%E6%9C%AC%E6%9C%BA%20Shell/Untitled%204.avif)
    
7. 在 SSH 至本机前，要先找一下 Docker 容器的网关地址是什么。在 属性 - 网络设置 - docker0 可以看到。我的是 10.0.5.1。
    
    ![](https://img.flinty.moe/blog/posts/2023/01/NAS%20%E5%8F%AA%E7%94%A8%E5%90%8E%E5%8F%B0%E7%BD%91%E9%A1%B5%20SSH%20%E6%9C%AC%E6%9C%BA%20Shell/Untitled%205.avif)
    
8. 回到刚才的终端，ssh 这个地址，就进入到了 NAS 自己的 shell 了。
    
    ![](https://img.flinty.moe/blog/posts/2023/01/NAS%20%E5%8F%AA%E7%94%A8%E5%90%8E%E5%8F%B0%E7%BD%91%E9%A1%B5%20SSH%20%E6%9C%AC%E6%9C%BA%20Shell/Untitled%206.avif)
    

### Synology

1. 在 Docker - 注册表 中搜索 `arangodb/ssh-client` 并双击，选择 latest 下载镜像
    
    ![](https://img.flinty.moe/blog/posts/2023/01/NAS%20%E5%8F%AA%E7%94%A8%E5%90%8E%E5%8F%B0%E7%BD%91%E9%A1%B5%20SSH%20%E6%9C%AC%E6%9C%BA%20Shell/Untitled%207.avif)
    
2. 下载完后在 映像 里找到，双击启动，网络选择默认的 bridge
    
    ![](https://img.flinty.moe/blog/posts/2023/01/NAS%20%E5%8F%AA%E7%94%A8%E5%90%8E%E5%8F%B0%E7%BD%91%E9%A1%B5%20SSH%20%E6%9C%AC%E6%9C%BA%20Shell/Untitled%208.avif)
    
    高级设置 - 执行命令 - 命令 这里改成 `/bin/sh`
    
    ![](https://img.flinty.moe/blog/posts/2023/01/NAS%20%E5%8F%AA%E7%94%A8%E5%90%8E%E5%8F%B0%E7%BD%91%E9%A1%B5%20SSH%20%E6%9C%AC%E6%9C%BA%20Shell/Untitled%209.avif)
    
    其他不用动，一路下一步启动容器
    
    ![](https://img.flinty.moe/blog/posts/2023/01/NAS%20%E5%8F%AA%E7%94%A8%E5%90%8E%E5%8F%B0%E7%BD%91%E9%A1%B5%20SSH%20%E6%9C%AC%E6%9C%BA%20Shell/Untitled%2010.avif)
    
3. 启动后，打开容器详情，在 终端机 就可以进入容器的 shell 了。
    
    💡 在老版本的 DSM 中默认可能没有 shell，需要手动创建，点击 新建 右侧的 通过命令启动，命令输入 `/bin/sh`
        
    ![](https://img.flinty.moe/blog/posts/2023/01/NAS%20%E5%8F%AA%E7%94%A8%E5%90%8E%E5%8F%B0%E7%BD%91%E9%A1%B5%20SSH%20%E6%9C%AC%E6%9C%BA%20Shell/Untitled%2011.avif)
    
    💡 如果是自己使用 nginx 将 NAS 后台反代到公网，请务必确保已启用 websocket 支持
        
4. 连接前，需要找一下容器网关地址，在 网络 - bridge 展开后可以看到。我这边是 172.17.0.1。
    
    ![](https://img.flinty.moe/blog/posts/2023/01/NAS%20%E5%8F%AA%E7%94%A8%E5%90%8E%E5%8F%B0%E7%BD%91%E9%A1%B5%20SSH%20%E6%9C%AC%E6%9C%BA%20Shell/Untitled%2012.avif)
    
5. 在刚才打开的容器 Shell 窗口中 ssh 网关，就连接到了本机 Shell
    
    ![](https://img.flinty.moe/blog/posts/2023/01/NAS%20%E5%8F%AA%E7%94%A8%E5%90%8E%E5%8F%B0%E7%BD%91%E9%A1%B5%20SSH%20%E6%9C%AC%E6%9C%BA%20Shell/Untitled%2013.avif)
    
    💡 你的 SSH 端口有可能因为某次安全提示修改了端口。如果需要指定端口，在地址后面加上  `-p <端口号>` 即可
    
> Photo by [Thái An](https://unsplash.com/@johnn21?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
  