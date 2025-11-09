+++
author = "FlintyLemming"
title = "中心化逻辑部署 Easytier - 无 Web 管理"
slug = "1b37bda595c580c989edf890db8e0b19"
date = "2025-03-11"
description = "纯命令行启动组网"
categories = ["Network"]
tags = ["Easytier"]
image = "https://assets.mitsea.cn/blog/posts/2025/03/%E4%B8%AD%E5%BF%83%E5%8C%96%E9%80%BB%E8%BE%91%E9%83%A8%E7%BD%B2%20Easytier%20-%20%E6%97%A0%20Web%20%E7%AE%A1%E7%90%86/pawel-czerwinski-MIXZflFpQXk-unsplash.avif"
+++

## 部署逻辑

这个软件其实主打的是去中心化，但是如果从 netbird 或者 tailscale 迁移过来，就很不习惯。所以写一个教程按照中心化的使用逻辑去部署。

按中心化的使用逻辑，需要先部署一个中枢，用来管理所有节点，所有节点也要连接到这个机器上。

如果你部署 Headscale，那本质上就是一个 Web 控制台 + DERP 中继服务器；如果你部署 Netbird，那本质上就是 Web 控制台 + coturn 中继服务器。只不过你部署的时候是丢到一个 Compose 里一起跑的。

Easytier 也类似，只不过你需要分开部署。不过由于当前版本下，web 配置无法设置网络白名单和接受子网的白名单，这会导致你的中继节点只要被扫描到就可以被他人使用，你的子网路由也会默认下发到所有节点上，使用上有所不便，所以本版本教程是在无 web 管理的情形下进行配置。若要查看 web 控制台部署，请参阅[这篇文章](https://blog.mitsea.com/1a57bda595c580088006c17d6ba2a744/)

若要便捷地查看其他设备，你只需要任意节点使用 GUI 客户端，或是使用 web 配置，也可看到其他设备，只是不能管理他们。

本教程使用二进制程序启动加参数的形式进行配置，不使用配置文件。二进制的所有参数可以通过执行 `easytier --help` 查看说明。

## 添加中继设备

### 配置步骤

1. 首先你要想一个网络名，比如叫 mytier。这个名字会在下面启动中继服务器时设置为白名单。
2. 在你想要作为中继服务器上启动 easytier
    
    使用 `--network-name` 和 `--network-secret` 设置网络名称与密码，后续的节点也要使用这个网络名称和密码
    
    使用 `--ipv4` 设置你要组网的内网 IP
    
    使用 `--hostname` 来设置一个容易辨别的设备名称
    
    `--relay-network-whitelist` 可以说是必加的，否则如果这个节点直接暴露在公网，任何人都可以拿来做中转服务器
    
    `--manual-routes` 也是建议加上的，EasyTier 如果你设置了子网路由，他会把路由表自动下发到所有节点上，这与 Tailscale 的行为是正好相反的。--manual-routes 后不加任何选项则默认拒绝所有子网路由表，后续你要接受哪个子网网段的路由表就再添加即可
    
    ```bash
    ./easytier-core --network-name mytier \
    --network-secret passwd \
    --ipv4 192.168.99.1/24 \
    --hostname node1 \
    --manual-routes \
    --relay-network-whitelist mytier
    ```
    
3. 默认端口是 11010，记得开放端口，假设你公网 IP 是 11.22.33.44

## 添加其他设备

启动命令和中转服务器基本是一样的，只需要修改 `--ipv4` 和 `--hostname` 和添加中继节点信息

```bash
./easytier-core --network-name mytier \
--network-secret passwd \
--ipv4 192.168.99.2/24 \
--hostname node2 \
--manual-routes \
--relay-network-whitelist mytier \
--external-node tcp://11.22.33.44:11010
```

至此，完成了等效于 Netbird 或是 Tailscale 的添加两个设备的步骤

## 子网路由设置

需要再次提醒的是，本教程只是面向从 Netbird 或者 Tailscale 迁移过来的用户，所以下面的说明假设你已经熟悉 Netbird 或者 Tailscale 的子网路由功能

1. 在你需要共享的局域网内按照上面的步骤安装并配置好 Easytier
2. 启动参数增加 `--proxy-networks` ，比如你要下发你当前所在的内网 192.168.5.0/24 和 192.168.2.0/24 给其他节点，那你的启动命令就变成
    
    ```bash
    ./easytier-core --network-name mytier \
    --network-secret passwd \
    --ipv4 192.168.99.2/24 \
    --hostname node2 \
    --manual-routes \
    --relay-network-whitelist mytier \
    --proxy-networks 192.168.5.0/24 \
    --proxy-networks 192.168.2.0/24 \
    --external-node tcp://11.22.33.44:11010
    ```
    
    默认情况下，他会把路由表下发到所有其他设备上，不过由于在前面我推荐你使用不带参数的 `--manual-routes` 启动客户端，所以现在其他客户端默认是不会接受下发的路由的。
    
3. 若要在别的设备上接收这条路由，修改 `--manual-routes` 选项的参数即可，比如你要接受 192.168.5.0/24 和 192.168.2.0/24 的路由，那启动命令就改成
    
    ```bash
    ./easytier-core --network-name mytier \
    --network-secret passwd \
    --ipv4 192.168.99.3/24 \
    --hostname node3 \
    --manual-routes 192.168.5.0/24 192.168.2.0/24 \
    --relay-network-whitelist mytier \
    --external-node tcp://11.22.33.44:11010
    ```
    
4. 如果做网关的话，转发和防火墙可以酌情配置，这些和 netbird 与 tailscale 是一致的：开启 ipv4 转发
    
    在 `/etc/sysctl.conf` ****文件中添加或修改以下行：
    
    ```
    net.ipv4.ip_forward = 1
    ```
    
    然后运行 `sudo sysctl -p` 来应用更改
    
5. 配置 NAT 规则
    
    ```bash
    sudo iptables -I FORWARD -i eth0 -j ACCEPT
    sudo iptables -I FORWARD -o eth0 -j ACCEPT
    sudo iptables -t nat -I POSTROUTING -o eth0 -j MASQUERADE
    sudo iptables -I FORWARD -i tun0 -j ACCEPT
    sudo iptables -I FORWARD -o tun0 -j ACCEPT
    sudo iptables -t nat -I POSTROUTING -o tun0 -j MASQUERADE
    ```
    

## 保活进程

由于不使用 web 管理，所以只需要保活 easytire-core 这一个程序

### 注册为 systemd 服务

1. 移动二进制文件并设置执行权限
    
    ```bash
    sudo mv easytier-core /usr/local/bin/easytier-core
    sudo chmod +x /usr/local/bin/easytier-core
    ```
    
2. 创建 systemd 服务单元文件
    
    ```bash
    sudo nano /etc/systemd/system/easytier-core.service
    ```
    
    内容如下，就是把启动的参数重新写一遍
    
    ```
    [Unit]
    Description=easytier-core Service
    After=network.target
    
    [Service]
    Type=simple
    ExecStart=/usr/local/bin/easytier-core --network-name mytier --network-secret passwd --ipv4 192.168.99.1/24 --hostname node1 --manual-routes --relay-network-whitelist mytier
    User=root
    Restart=on-failure
    RestartSec=5
    
    [Install]
    WantedBy=multi-user.target
    
    ```
    
3. 设置权限与加载服务
    
    保存并退出编辑器后，执行以下命令让 systemd 重新加载服务配置：
    
    ```
    sudo systemctl daemon-reload
    
    ```
    
    让该服务在开机时自动启动：
    
    ```
    sudo systemctl enable easytier-core
    
    ```
    
    启动该服务：
    
    ```
    sudo systemctl start easytier-core
    
    ```
    
    你可以使用下面的命令查看服务状态和日志：
    
    ```
    systemctl status easytier-core
    journalctl -u easytier-core -f
    
    ```

### 使用 docker 部署

参考官方文档 https://easytier.cn/guide/installation.html 修改这里的启动参数即可

![](https://assets.mitsea.cn/blog/posts/2025/03/%E4%B8%AD%E5%BF%83%E5%8C%96%E9%80%BB%E8%BE%91%E9%83%A8%E7%BD%B2%20Easytier%20-%20%E6%97%A0%20Web%20%E7%AE%A1%E7%90%86/image.avif)

```bash
command: --network-name mytier --network-secret passwd --ipv4 192.168.99.1/24 --hostname node1 --manual-routes --relay-network-whitelist mytier
```

> Photo by [Pawel Czerwinski](https://unsplash.com/@pawel_czerwinski?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/a-black-and-white-photo-of-a-bunch-of-flowers-MIXZflFpQXk?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)
      