+++
author = "FlintyLemming"
title = "搭建 ss-server 返回家中网络"
slug = "72397172ace045e79cb399b83cee3673"
date = "2020-11-23"
description = ""
categories = ["HomeLab", "Network"]
tags = ["家庭宽带"]
image = "https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/10/%E6%90%AD%E5%BB%BA%20ss-server%20%E8%BF%94%E5%9B%9E%E5%AE%B6%E4%B8%AD%E7%BD%91%E7%BB%9C/title.avif"
+++

家里开了公网 IP 后，在外面有时需要返回到家里的局域网中，不然每个服务都 NAT 端口出去，既麻烦也不安全。

目前用的比较多的方法是 VPN，但这种方法最大的问题就是你一旦打开了 VPN，那其他的链接也会受影响。个人感觉最方便的就是使用 ss-server。下面介绍下搭建和使用。

首先我们要明确达到什么效果，比如我们要访问内网中的 192.168.3.8 这个地址，如果我们在家，就应该能直接连接，如果我们在外面，直接访问这个地址也应该能访问到，并且不影响其他地址的访问。比如我们在公司，公司网关是 192.168.4.1（简单举个例子，公司网络一般都比较复杂，掩码不太可能是 255.255.255.0），那我应该同时既能通过 192.168.4.3 访问公司内网的服务，也应该能通过 192.168.3.8 访问家里的服务，不能冲突。

## 搭建服务端

搭建我使用 Docker 进行搭建，由于我的 Windows 电脑不关机，所以我在 Windows 上的 Docker 部署。群晖也有 Docker，同样可以部署。此外，不少路由器的第三方系统中的软件中心也会提供 ss-server 的工具。（假设你已经会基本的 Docker 用法，下面不会讲的特别详细，如果需要，请评论）

### Docker 配置

Docker 镜像我这边使用的是 gists/shadowsocks-libev。

环境变量注意三个地方，SERVER_PORT 是端口号、METHOD 是加密方式、PASSWORD 是密码，根据自己的需要修改。

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/10/%E6%90%AD%E5%BB%BA%20ss-server%20%E8%BF%94%E5%9B%9E%E5%AE%B6%E4%B8%AD%E7%BD%91%E7%BB%9C/1.avif)

端口映射这里，容器端口填写刚才 SERVER_PORT 里的，Published 端口填写需要转发出去的端口，后面路由器 NAT 要用到。如果小白不熟悉，就 SERVER_PORT、Docker Port、Published Port 三者一致也可。

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/10/%E6%90%AD%E5%BB%BA%20ss-server%20%E8%BF%94%E5%9B%9E%E5%AE%B6%E4%B8%AD%E7%BD%91%E7%BB%9C/2.avif)

其他就没了，不需要设置路径映射。

### 路由器 NAT

这个就应该都会了，我们只需要把上面的 Published Port 在路由后台的端口转发（NAT）功能里转发到公网即可。

## 客户端配置

上面配置完后，我们现在在外面使用任何一个支持 ss 协议的软件，地址填写 `<DDNS 地址>:<上面转发的端口号>`，密码、加密方式填写环境变量里设置的，就可以连上家里的网络了。

主要是我们要做分流，我们要达到开头说的效果，这就需要一个支持分流和策略组的代理工具了，Windows 有 Clash、iOS 有 Surge 和 Quantumult、macOS 有 Clash 和 Surge。

### iOS - Quantumult X

1. 主菜单 - 节点 - 添加 可以把我们家里的节点先添加进来，比如说我这里就叫 Home

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/10/%E6%90%AD%E5%BB%BA%20ss-server%20%E8%BF%94%E5%9B%9E%E5%AE%B6%E4%B8%AD%E7%BD%91%E7%BB%9C/3.avif)

2. 然后编辑配置文件，在 [policy] 下新增一条根据 SSID 类型判断的策略

    ```bash
    ssid=backHome, Home, Home, FlintyLemming-Router:DIRECT
    ```

    简单解释下什么意思，ssid=<策略名>, <蜂窝网走的代理>, <Wi-Fi下走的代理>, <特定 Wi-Fi 名下走的代理>。那我这个的意思就是，蜂窝网和 Wi-Fi 下默认都走刚才添加的 Home 代理，但在家里的路由器下（名叫 FlintyLemming-Router）走直连。

3. 最后在 [filter_local] 下添加一条规则即可

    ```bash
    ip-cidr, 192.168.3.0/24, backHome
    ```

    简单解释下中间的 IP 怎么来的，192.168.3.0 是根据我的网关来的（一般就是你路由器地址），我的网关地址是 192.168.3.1，那就填 192.168.3.0。 24 是子网掩码，如果你的子网掩码是 255.255.255.0（简单来说就是你网络的设备 IP 地址只有最后一位是变的），那就是 3x8 = 24。如果子网掩码是 255.255.0.0，那就是 2x8 = 16。

### macOS - Surge

> 你这 SSID 都是无线，那有线咋办呢

不慌，这就要请到 Surge 了，Surge 的 SSID 策略组不仅能够根据 SSID、BSSID 判断，也可以根据网关地址判断，这样有线环境也搞定了。

1. 首先我们还是要添加代理

    ```bash
    Home = ss, <ddns地址>, <转发的端口>, encrypt-method=<设置的加密方式>, password=<密码>
    ```

2. 然后同样的，新增策略组

    ```bash
    isHome = ssid, default = Home, "FlintyLemming-Router" = DIRECT, 192.168.3.1 = DIRECT
    ```

    Surge 这边看的就很清楚了，除了 SSID 叫 FlintyLemming-Router 的无线，以及网关是 192.168.3.1 的有线走直连，其他都走 Home 代理。

3. 最后添加分流规则

    ```bash
    IP-CIDR,192.168.3.0/24,isHome
    ```

### Windows

由于目前 Clash 尚不支持根据网关地址判断的策略，只能设置一个手动的策略组，需要的时候打开。不过 Proxifier 4 支持根据 Interface 判断。

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/10/%E6%90%AD%E5%BB%BA%20ss-server%20%E8%BF%94%E5%9B%9E%E5%AE%B6%E4%B8%AD%E7%BD%91%E7%BB%9C/4.avif)

> Photo by [JJ Ying](https://unsplash.com/@jjying?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/network?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
