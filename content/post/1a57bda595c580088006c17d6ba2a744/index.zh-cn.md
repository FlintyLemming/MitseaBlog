+++
author = "FlintyLemming"
title = "中心化逻辑部署 Easytier"
slug = "1a57bda595c580088006c17d6ba2a744"
date = "2025-02-28"
description = "官方文档写的有点散，感觉从同类软件切过来不好上手"
categories = ["Network"]
tags = ["Easytier"]
image = "https://hf-image.mitsea.com:8840/blog/posts/2025/02/%E4%B8%AD%E5%BF%83%E5%8C%96%E9%80%BB%E8%BE%91%E9%83%A8%E7%BD%B2%20Easytier/the-chaffins-1KDauE4oF0o-unsplash.avif"
+++

## 部署逻辑

这个软件其实主打的是去中心化，但是如果从 netbird 或者 tailscale 迁移过来，就很不习惯。所以写一个教程按照中心化的使用逻辑去部署。

按中心化的使用逻辑，需要先部署一个中枢，用来管理所有节点，所有节点也要连接到这个机器上。

如果你部署 Headscale，那本质上就是一个 Web 控制台 + DERP 中继服务器；如果你部署 Netbird，那本质上就是 Web 控制台 + coturn 中继服务器。只不过你部署的时候是丢到一个 Compose 里一起跑的。

Easytier 也类似，只不过你需要分开部署。下面的步骤中，“管理后台配置”一节对应的就是 Web 控制台。“中继服务器配置”对应的就是 DERP 或是 coturn 的配置。

> 💡 请注意，当前版本下，使用 web 控制台部署设备，即使用 --config-server 启动会使得所有其他启动参数失效，你将会无法使用以下功能：手动选择接受的路由、网络白名单、rpc 包转发
## 管理后台配置

### 说明

1. 管理后台用的 easytier-web 这个二进制文件启动
2. 管理后台运行时启动了两个服务：config-server 和 api-server
3. config-server 是你节点需要连接的，类似于你用 `tailscale up` 的 `--login-server` 参数，或是 `netbird up` 的 `--management-url`
4. api-server 就是官方[这个 Dashboard](https://easytier.cn/web) 的后端
5. 自建的话就是自建这个后端，然后前台 Dashboard 网页也是需要部署的，你的 API 地址和前端的根域名必须要一致
6. 自建后端 + Dashboard 就等于 Netbird、Tailscale 那个 Web 管理页面，可以管理设备，设置子网路由

### 具体步骤

**部署后端**

1. 下载 GitHub Release 里的压缩包，里面有一个 easytier-web
2. 执行 `sudo chmod +x easytier-web` 使其可被执行后，执行 `./easytier-web --help` 后可以看到参数说明，你可以修改 config-server 和 api-server 各自的端口
    
    ![](https://hf-image.mitsea.com:8840/blog/posts/2025/02/%E4%B8%AD%E5%BF%83%E5%8C%96%E9%80%BB%E8%BE%91%E9%83%A8%E7%BD%B2%20Easytier/image.avif)
    
    默认直接运行 `./easytier-web` 即可，后续测试没问题后再部署成服务
    
3. 部署完后需要对 api-server 这个 RESTful API 进行反代 https 加密，套上你自己的域名
4. 至此，你部署出来了两个服务
    
    
    | 服务 | 说明 | 端口 | 协议 |
    | --- | --- | --- | --- |
    | api-server | 后面前端需要连接的 | 默认 11211，tcp，反代后那就是你自己 https 的端口 | tcp |
    | config-server | 后续节点需要链接的，需要你服务器防火墙单独开放这个端口 | 默认 22020，udp | udp |

**部署前端**

1. 拉下代码仓库，进入 easytier-web/frontend 文件夹
2. 执行下面两个命令编译出 html 文件（需要你有前端编译环境，若没有自行安装 node.js）
    
    ```bash
    pnpm -r install
    pnpm -r build
    ```
    
3. 把 easytier-web/frontend/dict 里的 html 文件部署到服务器上就可以用了。注意你前台的根域名和 API 后端的根域名要一样
    
    ![](https://hf-image.mitsea.com:8840/blog/posts/2025/02/%E4%B8%AD%E5%BF%83%E5%8C%96%E9%80%BB%E8%BE%91%E9%83%A8%E7%BD%B2%20Easytier/image%201.avif)
    
4. 注册并登陆，就能看到控制台了
    
    ![](https://hf-image.mitsea.com:8840/blog/posts/2025/02/%E4%B8%AD%E5%BF%83%E5%8C%96%E9%80%BB%E8%BE%91%E9%83%A8%E7%BD%B2%20Easytier/image%202.avif)
    

## 添加中继设备

### 说明

需要再次说明的是，由于 web 配置的方式无法设置网络名称白名单，所以任何人扫描到你这个服务器，都可以拿来作为中继服务器

### 配置步骤

1. 在你想要作为中继服务器上启动 easytier 并连接上刚才部署的 config-server。config-server 斜杠后面是用户名，假设你 config-server IP 是 1.2.3.4，用户名是 abc，那就执行以下命令
    
    ```bash
    ./easytier-core --config-server udp://1.2.3.4:22020/abc
    ```
    
    没有问题的话会提示连接成功
    
    ![](https://hf-image.mitsea.com:8840/blog/posts/2025/02/%E4%B8%AD%E5%BF%83%E5%8C%96%E9%80%BB%E8%BE%91%E9%83%A8%E7%BD%B2%20Easytier/image%203.avif)
    
3. 默认端口是 11010，记得开放端口，修改的话参考官方文档
4. 去到 Dashboard 那边，这里就能看到设备了。点设备右边的齿轮
    
    ![](https://hf-image.mitsea.com:8840/blog/posts/2025/02/%E4%B8%AD%E5%BF%83%E5%8C%96%E9%80%BB%E8%BE%91%E9%83%A8%E7%BD%B2%20Easytier/image%204.avif)
    
5. 点 Create
    
    ![](https://hf-image.mitsea.com:8840/blog/posts/2025/02/%E4%B8%AD%E5%BF%83%E5%8C%96%E9%80%BB%E8%BE%91%E9%83%A8%E7%BD%B2%20Easytier/image%205.avif)
    
6. 填写信息
    
    ![](https://hf-image.mitsea.com:8840/blog/posts/2025/02/%E4%B8%AD%E5%BF%83%E5%8C%96%E9%80%BB%E8%BE%91%E9%83%A8%E7%BD%B2%20Easytier/image%20x.avif)
    
7. 加完后，这里选择刚才创建的网络
    
    ![](https://hf-image.mitsea.com:8840/blog/posts/2025/02/%E4%B8%AD%E5%BF%83%E5%8C%96%E9%80%BB%E8%BE%91%E9%83%A8%E7%BD%B2%20Easytier/image%207.avif)
    
8. 加完后就可以看到这里是本机
    
    ![](https://hf-image.mitsea.com:8840/blog/posts/2025/02/%E4%B8%AD%E5%BF%83%E5%8C%96%E9%80%BB%E8%BE%91%E9%83%A8%E7%BD%B2%20Easytier/image%208.avif)
    

## 添加其他设备

1. 启动命令和中转服务器是一样的
    
    ```bash
    ./easytier-core --config-server udp://1.2.3.4:22020/abc
    ```
    
2. 然后在 Dashboard 里就可以看到另一个机器了，点击它右边的齿轮
    
    ![](https://hf-image.mitsea.com:8840/blog/posts/2025/02/%E4%B8%AD%E5%BF%83%E5%8C%96%E9%80%BB%E8%BE%91%E9%83%A8%E7%BD%B2%20Easytier/image%209.avif)
    
3. 这边无法直接选择刚才的网络，还是要点右边的 Create 手动添加一下
    
    ![](https://hf-image.mitsea.com:8840/blog/posts/2025/02/%E4%B8%AD%E5%BF%83%E5%8C%96%E9%80%BB%E8%BE%91%E9%83%A8%E7%BD%B2%20Easytier/image%2010.avif)
    
4. 填写一样的网络名称、网络密码和公共服务器，IP 地址换一个同网段的另一个 IP 即可
    
    ![](https://hf-image.mitsea.com:8840/blog/posts/2025/02/%E4%B8%AD%E5%BF%83%E5%8C%96%E9%80%BB%E8%BE%91%E9%83%A8%E7%BD%B2%20Easytier/image%2011.avif)
    
    注意 网络方式 那里不要选手动，不然即便填了一样的网络名称和密码，两个机器之间互相也是看不到的
    
5. 加完后，这里选择刚才创建的网络
    
    ![](https://hf-image.mitsea.com:8840/blog/posts/2025/02/%E4%B8%AD%E5%BF%83%E5%8C%96%E9%80%BB%E8%BE%91%E9%83%A8%E7%BD%B2%20Easytier/image%2012.avif)
    
6. 可以看到两个设备之间可以通信了
    
    ![](https://hf-image.mitsea.com:8840/blog/posts/2025/02/%E4%B8%AD%E5%BF%83%E5%8C%96%E9%80%BB%E8%BE%91%E9%83%A8%E7%BD%B2%20Easytier/image%2013.avif)
    

至此，完成了等效于 Netbird 或是 Tailscale 的添加两个设备的步骤

## 子网路由设置

需要再次提醒的是，本教程只是面向从 Netbird 或者 Tailscale 迁移过来的用户，所以下面的说明假设你已经熟悉 Netbird 或者 Tailscale 的子网路由功能。以及通过 Web 控制台管理无法控制接受的子网，默认情况下，他会把路由表下发到所有其他设备上。

1. 在你需要共享的局域网内按照上面的步骤安装并配置好 Easytier
2. 在网页控制台里打开这个设备的设置，编辑网络，展开 高级设置 后在 子网代理CIDR 里填写内网网段即可
    
    ![](https://hf-image.mitsea.com:8840/blog/posts/2025/02/%E4%B8%AD%E5%BF%83%E5%8C%96%E9%80%BB%E8%BE%91%E9%83%A8%E7%BD%B2%20Easytier/image%2014.avif)
    
3. 填好后，点击运行网络，就会把新配置下发到客户端上，并自动应用新配置。
    
    ![](https://hf-image.mitsea.com:8840/blog/posts/2025/02/%E4%B8%AD%E5%BF%83%E5%8C%96%E9%80%BB%E8%BE%91%E9%83%A8%E7%BD%B2%20Easytier/image%2015.avif)
 
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

首先梳理一下目前跑了哪几个项目

| 名称 | 二进制程序 | 设备 |
| --- | --- | --- |
| 网页服务 api-server + config-server | easytier-web | A |
| 自建公共服务转发机 | easytier-core | B（也可以跟 A 放一起） |
| 其他入网设备 | easytier-core | C |

### 注册为 systemd 服务

所以我们需要在设备 A 上将 `./easytier-web` 这个命令注册为服务，在 B 和 C 上将 `./easytier-core --config-server udp://1.2.3.4:22020/abc` 这个命令注册为服务

**注册 easytier-web**

1. 移动二进制文件并设置执行权限
    
    ```
    sudo mv easytier-web /usr/local/bin/easytier-web
    sudo chmod +x /usr/local/bin/easytier-web
    
    ```
    
2. 创建 systemd 服务单元文件
    
    ```
    sudo nano /etc/systemd/system/easytier-web.service
    
    ```
    
    内容如下
    
    ```
    [Unit]
    Description=easytier-web Service
    After=network.target
    
    [Service]
    Type=simple
    ExecStart=/usr/local/bin/easytier-web
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
    sudo systemctl enable easytier-web
    
    ```
    
    启动该服务：
    
    ```
    sudo systemctl start easytier-web
    
    ```
    
    你可以使用下面的命令查看服务状态和日志：
    
    ```
    systemctl status easytier-web
    journalctl -u easytier-web -f
    
    ```
    

**注册 easytier-core**

与 easytier-web 同理

1. 移动二进制文件并设置执行权限
    
    ```bash
    sudo mv easytier-core /usr/local/bin/easytier-core
    sudo chmod +x /usr/local/bin/easytier-core
    
    ```
    
2. 创建 systemd 服务单元文件
    
    ```bash
    sudo nano /etc/systemd/system/easytier-core.service
    
    ```
    
    内容如下
    
    ```
    [Unit]
    Description=easytier-core Service
    After=network.target
    
    [Service]
    Type=simple
    ExecStart=/usr/local/bin/easytier-core --config-server udp://1.2.3.4:22020/abc
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

**注册 easytier-web**

运行下面的命令启动

```bash
docker run -d --entrypoint easytier-web -v /yourpath/data:/app -p 11211:11211 -p 22020:22020/udp easytier/easytier:latest
```

主要就是重新指定了 entrypoint，使用 easytier-web 启动

-v 路径映射根据自己的情况修改，/app 不要改，这个可以在 docker hub 上看到他默认的 workdir

![](https://hf-image.mitsea.com:8840/blog/posts/2025/02/%E4%B8%AD%E5%BF%83%E5%8C%96%E9%80%BB%E8%BE%91%E9%83%A8%E7%BD%B2%20Easytier/image%2016.avif)

**注册 easytier-core**

参考官方文档 https://easytier.cn/guide/installation.html 修改这里的启动参数即可

![](https://hf-image.mitsea.com:8840/blog/posts/2025/02/%E4%B8%AD%E5%BF%83%E5%8C%96%E9%80%BB%E8%BE%91%E9%83%A8%E7%BD%B2%20Easytier/image%2017.avif)

```
command: --config-server udp://1.2.3.4:22020/abc
```

> Photo by [The Chaffins](https://unsplash.com/@thechaffins?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/a-large-rock-in-the-middle-of-a-forest-1KDauE4oF0o?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)
      