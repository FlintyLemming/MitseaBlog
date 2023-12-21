+++
author = "FlintyLemming"
title = "群晖 DSM 7.x 安装 哪吒监控 Agent"
slug = "3929551d08bd4bb0a8baa453e2d92b0c"
date = "2022-10-08"
description = ""
categories = ["HomeLab"]
tags = ["Synology", "哪吒监控"]
image = "https://img.mitsea.com/blog/posts/2022/10/%E7%BE%A4%E6%99%96%20DSM%207.x%20%E5%AE%89%E8%A3%85%20%E5%93%AA%E5%90%92%E7%9B%91%E6%8E%A7%20Agent/title.jpg?x-oss-process=style/ImageCompress"
+++

## 下载 Agent

1. 先去哪吒的下载页下载符合自己群晖 CPU 架构的 Agent 客户端。我是 DS420+，Intel CPU，下载 nezha-agent_linux_amd64.zip。

    <https://github.com/nezhahq/agent/releases>

2. 解压后，把里面的二进制文件随便放到一个地方

    ![](https://img.mitsea.com/blog/posts/2022/10/%E7%BE%A4%E6%99%96%20DSM%207.x%20%E5%AE%89%E8%A3%85%20%E5%93%AA%E5%90%92%E7%9B%91%E6%8E%A7%20Agent/1.png?x-oss-process=style/ImageCompress)

3. SSH 到群晖，使用 `sudo -i` 切换到 root 账号
4. 找到刚才放进去的文件，共享文件夹一般在 /volume1 下面。刚才我是放在了 AppData 这个共享文件夹里，那二进制文件就在 /volume1/AppData 下

    ![](https://img.mitsea.com/blog/posts/2022/10/%E7%BE%A4%E6%99%96%20DSM%207.x%20%E5%AE%89%E8%A3%85%20%E5%93%AA%E5%90%92%E7%9B%91%E6%8E%A7%20Agent/2.png?x-oss-process=style/ImageCompress)

## 测试启动

首先要手动执行下二进制文件，确认使用正常。

1. 在 Dashboard 里创建一个新设备，获取链接密码
2. 进到 agent 所在文件夹后，通过执行 ./nezha-agent 可以查看使用帮助

    ![](https://img.mitsea.com/blog/posts/2022/10/%E7%BE%A4%E6%99%96%20DSM%207.x%20%E5%AE%89%E8%A3%85%20%E5%93%AA%E5%90%92%E7%9B%91%E6%8E%A7%20Agent/3.png?x-oss-process=style/ImageCompress)

3. 基本上指定一下服务器地址和密码即可，命令为

    ```bash
    ./nezha-agent -s <Dashboard服务器地址>:<端口> -p <连接密码>
    ```

4. 执行后，没报错，并且在 Web 上能看到信息就可以

    ![](https://img.mitsea.com/blog/posts/2022/10/%E7%BE%A4%E6%99%96%20DSM%207.x%20%E5%AE%89%E8%A3%85%20%E5%93%AA%E5%90%92%E7%9B%91%E6%8E%A7%20Agent/4.png?x-oss-process=style/ImageCompress)

5. 记下自己的执行命令，并把 `./` 替换成绝对路径，比如我的文件放在 `/volume1/AppData` 下，那我的执行命令就是

    ```bash
    /volume1/AppData/nezha-agent -s <Dashboard服务器地址>:<端口> -p <连接密码>
    ```

## 配置服务

直接执行命令虽然能用，但是一关窗口就停止运行了。丢到 screen 里也不行，因为 Agent 会自动更新，更新时会停止进程。DSM 7 内置 systemctl 命令，可以很方便设置为系统服务，保活进程。

1. 在 `/usr/lib/systemd/system` 下创建一个 `nezha-agent.service` 文本文件

    ```bash
    vim /usr/lib/systemd/system/nezha-agent.service
    ```

    内容如下

    ```bash
    [Unit]
    Description=Nezha Agent
    After=syslog.target
    
    [Service]
    Type=simple
    User=root
    Group=root
    ExecStart=<刚才记下的执行命令>
    Restart=always
    
    [Install]
    WantedBy=multi-user.target
    ```

2. 加载配置文件

    ```bash
    systemctl daemon-reload
    ```

3. 启动服务

    ```bash
    systemctl enable nezha-agent
    systemctl start nezha-agent
    ```

4. 查看状态

    ```bash
    systemctl status nezha-agent
    ```

    ![](https://img.mitsea.com/blog/posts/2022/10/%E7%BE%A4%E6%99%96%20DSM%207.x%20%E5%AE%89%E8%A3%85%20%E5%93%AA%E5%90%92%E7%9B%91%E6%8E%A7%20Agent/5.png?x-oss-process=style/ImageCompress)

> Photo by [Steve Johnson](https://unsplash.com/@steve_j?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
