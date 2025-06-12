+++
author = "FlintyLemming"
title = "Netbird 子网路由设置 （处理不便安装的设备）"
slug = "77af4e22a9e44f7f8c2803edc33ab33b"
date = "2024-03-11"
description = ""
categories = ["HomeLab", "Network"]
tags = ["异地组网", "Tailscale", "Netbird"]
image = "https://img.flinty.moe/blog/posts/2024/03/Netbird%20%E5%AD%90%E7%BD%91%E8%B7%AF%E7%94%B1%E8%AE%BE%E7%BD%AE%20%EF%BC%88%E5%A4%84%E7%90%86%E4%B8%8D%E4%BE%BF%E5%AE%89%E8%A3%85%E7%9A%84%E8%AE%BE%E5%A4%87%EF%BC%89/martin-katler-eVk59Ks4k2U-unsplash.avif"
+++

上一期简单介绍了 Netbird 的子网路由的配置，相比与 Tailscale 真的是简单很多。但是还遗留一个问题，就是如果设备不方便或者不想安装 Netbird 应该怎么办。其实不影响的，两个内网只要分别在一台设备上安装 Netbird，这两个内网的所有设备其实都可以互访。比如在下图中，让设备 A1 直接访问 B1（红色箭头）。当然，也可以直接访问 Netbird 内网中的其他设备（橙色箭头）。

![](https://img.flinty.moe/blog/posts/2024/03/Netbird%20%E5%AD%90%E7%BD%91%E8%B7%AF%E7%94%B1%E8%AE%BE%E7%BD%AE%20%EF%BC%88%E5%A4%84%E7%90%86%E4%B8%8D%E4%BE%BF%E5%AE%89%E8%A3%85%E7%9A%84%E8%AE%BE%E5%A4%87%EF%BC%89/Netbird_%25E5%2586%2585%25E7%25BD%2591%25E8%25B7%25AF%25E7%2594%25B1.avif)

这样在内网其他设备上即便不安装任何组网工具，也可以无缝访问多个内网

![](https://img.flinty.moe/blog/posts/2024/03/Netbird%20%E5%AD%90%E7%BD%91%E8%B7%AF%E7%94%B1%E8%AE%BE%E7%BD%AE%20%EF%BC%88%E5%A4%84%E7%90%86%E4%B8%8D%E4%BE%BF%E5%AE%89%E8%A3%85%E7%9A%84%E8%AE%BE%E5%A4%87%EF%BC%89/Untitled.avif)

## 准备工作

在两个内网中分别选取一个设备安装 Netbird 并配置为路由，让两个设备分别可以访问对方内网所有设备。在新版中 Netbird 可以选取 Windows 设备作为路由，不过我感觉 Windows 的网络栈比较灵车，所以我还是建议使用一台 Linux 设备。

这次配置以 Ubuntu 系统为例，配置区域 A 的内网设备可以访问区域 B 的内网设备，反向同理。

## 配置 Netbird 路由

配置方法在上一集已经说过了，这里简单提一下，最近 Netbird UI 改了一次。

1 处填写目标区域内网 IP 地址段

2 处填写目标区域内网安装 Netbird 的设备

3 处填写来源区域内网安装 Netbird 的设备所在的 Group

![](https://img.flinty.moe/blog/posts/2024/03/Netbird%20%E5%AD%90%E7%BD%91%E8%B7%AF%E7%94%B1%E8%AE%BE%E7%BD%AE%20%EF%BC%88%E5%A4%84%E7%90%86%E4%B8%8D%E4%BE%BF%E5%AE%89%E8%A3%85%E7%9A%84%E8%AE%BE%E5%A4%87%EF%BC%89/Untitled%201.avif)

这样的话，以开头的图为例，设备 A2 就可以访问区域 B 的所有内网设备了

## 配置静态路由

设置区域 A 的路由器，添加静态路由。目的是让这个内网设备访问区域 B 的内网网段时，不会被路由器丢弃，而是发送到 Netbird 设备上。具体配置如下：

![](https://img.flinty.moe/blog/posts/2024/03/Netbird%20%E5%AD%90%E7%BD%91%E8%B7%AF%E7%94%B1%E8%AE%BE%E7%BD%AE%20%EF%BC%88%E5%A4%84%E7%90%86%E4%B8%8D%E4%BE%BF%E5%AE%89%E8%A3%85%E7%9A%84%E8%AE%BE%E5%A4%87%EF%BC%89/Untitled%202.avif)

目的地址和子网掩码：填写区域 B 的内网网段，可以再加一条，填写 Netbird 的内网网段

下一跳：填写当前内网里安装 Netbird 设备的 IP 地址

出接口：LAN

## 配置 Netbird 设备

浏览器添加静态路由后，访问区域 B 的内网请求就被转发到了 Netbird 设备上。但是这个设备还要进行一些配置才能正确通过 Netbird 虚拟局域网转发流量。具体配置如下：

1. 确认 Netbird 创建的 TUN 所使用的 Interface 名称（一般是 wt0）以及默认联网网卡的 Interface 名称。你可以使用`ip addr show` 来确认
    
    ![](https://img.flinty.moe/blog/posts/2024/03/Netbird%20%E5%AD%90%E7%BD%91%E8%B7%AF%E7%94%B1%E8%AE%BE%E7%BD%AE%20%EF%BC%88%E5%A4%84%E7%90%86%E4%B8%8D%E4%BE%BF%E5%AE%89%E8%A3%85%E7%9A%84%E8%AE%BE%E5%A4%87%EF%BC%89/Untitled%203.avif)
    
    我这边就是 wt0 和 eth0，这两个 Interface 名称要记住，后面配置 iptables 要用。
    
2. 开启 ipv4 转发
    
    在 `/etc/sysctl.conf` 文件中添加或修改以下行：
    
    ```
    net.ipv4.ip_forward = 1
    ```
    
    然后运行 `sudo sysctl -p` 来应用更改
    
3. 配置 NAT 规则
    
    ```bash
    sudo iptables -I FORWARD -i eth0 -j ACCEPT
    sudo iptables -I FORWARD -o eth0 -j ACCEPT
    sudo iptables -t nat -I POSTROUTING -o eth0 -j MASQUERADE
    sudo iptables -I FORWARD -i wt0 -j ACCEPT
    sudo iptables -I FORWARD -o wt0 -j ACCEPT
    sudo iptables -t nat -I POSTROUTING -o wt0 -j MASQUERADE
    ```
    
    这个规则设置主要干了两件事：
    
    1. 使用 MASQUERADE 隐藏了从 eth0 出去的流量，让所有经过设备A的包看起来都是由本机发出的（wt0 同理）
    2. 允许 eth0 和 wt0 的流量转发
    
    若对安全性有要求，可以细化规则
    

至此设置完毕

> Photo by [Martin Katler](https://unsplash.com/@martinkatler?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/a-close-up-of-a-blue-and-purple-object-eVk59Ks4k2U?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)
  