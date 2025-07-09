+++
author = "FlintyLemming"
title = "给群晖 Docker 命令配置代理"
slug = "8c059ad1807e424d8f7059b330525443"
date = "2024-07-03"
description = ""
categories = ["Linux", "HomeLab"]
tags = ["群晖", "Docker"]
image = "https://img.mitsea.com/blog/posts/2024/07/%E7%BB%99%E7%BE%A4%E6%99%96%20Docker%20%E5%91%BD%E4%BB%A4%E9%85%8D%E7%BD%AE%E4%BB%A3%E7%90%86/karsten-winegeart-kBVAKM4f6V0-unsplash.avif"
+++

本方法在 SA6400 机型上测试可用，该方法只作用于 Docker 命令，`docker pull` 之类的，不作用于容器

1. 首先确保内网里有一个能够访问互联网的代理，随便什么设备，随便什么客户端，只要能起 http 代理就行，注意打开 LAN 访问权限

2. 执行下面的命令创建文件夹

    ```bash
    sudo mkdir -p /etc/systemd/system/pkg-ContainerManager-dockerd.service.d
    ```
3. 执行下面的命令创建配置文件

    ```bash
    sudo vi /etc/systemd/system/pkg-ContainerManager-dockerd.service.d/http-proxy.conf
    ```
4. 编辑内容如下，地址根据自己的 IP 和客户端端口修改

    ```bash
    [Service]
    Environment="HTTP_PROXY=http://192.168.2.52:6152"
    Environment="HTTPS_PROXY=http://192.168.2.52:6152"
    Environment="NO_PROXY=localhost,127.0.0.1"
    ```
5. 应用配置文件

    ```bash
    sudo systemctl daemon-reload
    ```
6. 重启服务，注意所有的容器都会被关闭

    ```bash
    sudo systemctl restart pkg-ContainerManager-dockerd.service
    ```

> Photo by [Karsten Winegeart](https://unsplash.com/@karsten116?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/a-black-and-white-photo-of-wavy-lines-kBVAKM4f6V0?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)
      