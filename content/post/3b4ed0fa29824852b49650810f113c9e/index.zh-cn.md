+++
author = "FlintyLemming"
title = "Tailscale 子网路由设置 （对比 Netbird）"
slug = "3b4ed0fa29824852b49650810f113c9e"
date = "2024-02-02"
description = ""
categories = ["HomeLab", "Network"]
tags = ["异地组网"]
image = "https://hf-public-source.mitsea.com:8840/images/blog/posts/2024/02/3b4ed0fa29824852b49650810f113c9e/d-p-hpsnTAl-6rQ-unsplash.avif"
+++

设置子网路由可以有效解决内网中有的设备不方便安装 Tailscale 但是又需要访问的问题，或者你单纯就是不想给那么多设备都装上 Tailscale 客户端。但是 Tailscale 的配置逻辑让这个功能的设置变得极其复杂，本文先介绍下具体设置步骤，再对比一下 Netbird 该功能的设置。

## Netbird 设置

假设现在有两个子网，一个是 192.168.5.0/24，有设备 A 192.168.5.5 和设备 B 192.168.5.6，A 不方便安装 Tailscale，但是 B 可以装。一个是你现在的网络 192.168.2.0/24，正在使用设备 C 192.168.2.7。

![](https://hf-public-source.mitsea.com:8840/images/blog/posts/2024/02/3b4ed0fa29824852b49650810f113c9e/%E6%9C%AA%E5%91%BD%E5%90%8D%E7%BB%98%E5%9B%BE.drawio%281%29.avif)

现在你要在设备 C 上可以直接通过输入 192.168.5.5 内网地址访问 A，应该如下设置：

### 子网路由设备的设置

1. 通过 SSH 连接设备 B 后，停止 Tailscale

    ```bash
    tailscale down
    ```

2. 重新启动 tailscale 并声明子网网段

    ```bash
    tailscale up --advertise-routes=192.168.5.0/24
    ```


### Tailscale 控制台设置

1. 打开 Tailscale 控制台网页，你会发现一个带有 Subnets 标志的设备。后面的感叹号说明这个子网声明没有被批准，点击下面的 Edit

    ![](https://hf-public-source.mitsea.com:8840/images/blog/posts/2024/02/3b4ed0fa29824852b49650810f113c9e/Untitled.avif)

2. 勾上后保存即可

    ![](https://hf-public-source.mitsea.com:8840/images/blog/posts/2024/02/3b4ed0fa29824852b49650810f113c9e/Untitled%201.avif)

    由于默认 ACL 设置里是所有规则都 allow，所以此时你的 Tailscale 所有设备都可以直接通过内网 IP 192.168.5.5 访问到设备。设备多的时候这显然是不行的，甚至有可能导致在某些设备上有冲突。所以我们还要设置权限。

3. 先把当前全 allow 的规则改掉

    ```yaml
    ## 把
    {"action": "accept", "src": ["*"], "dst": ["*:*"]},
    ## 改成
    {"action": "accept", "src": ["*"], "dst": ["100.0.0.0/8:*"]},
    ```

    这样保证设备之间仍然可以正常通过 Tailscale 分配的内网 IP 访问。

4. 添加规则，设置谁可以访问 192.168.5.0/24 子网

    ```yaml
    {
    	"action": "accept",
    	"src":    ["group:example@mail.com", "tag:example", "hosts_name", "xxx,xxx,xxx,xxx", "192.168.5.0/24"],
    	"dst":    ["192.168.5.0/24:*"],
    },
    ```

    这个 src 可以是 group、tag、hosts、IP 等，group、tag 和 hosts 都需要你在配置文件里定义好。但是一定要注意！src 里一定要把这个网段也写上，否则这个网段内的设备就访问不到了！！！

    ![](https://hf-public-source.mitsea.com:8840/images/blog/posts/2024/02/3b4ed0fa29824852b49650810f113c9e/Untitled%202.avif)

    如果是 tag，在这里声明后，还需要在设备列表里设置（真的麻烦）

    ![](https://hf-public-source.mitsea.com:8840/images/blog/posts/2024/02/3b4ed0fa29824852b49650810f113c9e/Untitled%203.avif)

    在本例中，我是给设备 C 设置了一个 tag，然后规则如下

    ![](https://hf-public-source.mitsea.com:8840/images/blog/posts/2024/02/3b4ed0fa29824852b49650810f113c9e/%E6%8D%95%E8%8E%B7.avif)


这样，设备 C 就可以直接访问设备 A 了

![](https://hf-public-source.mitsea.com:8840/images/blog/posts/2024/02/3b4ed0fa29824852b49650810f113c9e/Untitled%205.avif)

非常复杂，非常麻烦！下面看下 Netbird 的配置方法

## Netbird 配置方法

首先还是给不同的设备设置不同的 tags 便于后面的权限设置，直接在这里选择或者新增 tag，不需要修改任何配置文件

![](https://hf-public-source.mitsea.com:8840/images/blog/posts/2024/02/3b4ed0fa29824852b49650810f113c9e/Untitled%206.avif)

然后新增一条路由，在 Network Routes 里 Add route。依次填写名称、内网网段、子网路由设备、可以访问的设备组即可。

![](https://hf-public-source.mitsea.com:8840/images/blog/posts/2024/02/3b4ed0fa29824852b49650810f113c9e/Untitled%207.avif)

就结束了……

这才是人用的设置……
