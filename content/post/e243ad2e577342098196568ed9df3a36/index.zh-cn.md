+++
author = "FlintyLemming"
title = "QNAP 部署 哪吒监控 Agent"
slug = "e243ad2e577342098196568ed9df3a36"
date = "2022-10-24"
description = "QTS 5.10 的内核但是没用 SystemD 真是奇怪"
categories = ["HomeLab"]
tags = ["QNAP"]
image = "https://hf-image.mitsea.com:8840/blog/posts/2022/10/QNAP%20%E9%83%A8%E7%BD%B2%20%E5%93%AA%E5%90%92%E7%9B%91%E6%8E%A7%20Agent/title.avif"
+++

## 下载 Agent

1. 先去哪吒的下载页下载符合自己 QNAP CPU 架构的 Agent 客户端。我是 212p3，arm64 处理机，下载 nezha-agent_linux_arm64.zip。

    <https://github.com/naiba/nezha/releases>

2. 解压后，把里面的二进制文件随便放到一个地方

    ![](https://hf-image.mitsea.com:8840/blog/posts/2022/10/QNAP%20%E9%83%A8%E7%BD%B2%20%E5%93%AA%E5%90%92%E7%9B%91%E6%8E%A7%20Agent/Untitled.avif)

3. SSH 到 QNAP，使用 `sudo -i` 切换到 root 账号。默认进去有个菜单，选择退出即可回到命令行。
4. 找到刚才放进去的文件，共享文件夹一般在 /share 下面。刚才我是放在了个人账号的 home 共享文件夹里，那二进制文件就在 /share/homes/FlintyLemming 下

## 测试启动

首先要手动执行下二进制文件，确认使用正常。

1. 在 Dashboard 里创建一个新设备，获取链接密码
2. 进到 agent 所在文件夹后，通过执行 ./nezha-agent 可以查看使用帮助

    ![](https://hf-image.mitsea.com:8840/blog/posts/2022/10/QNAP%20%E9%83%A8%E7%BD%B2%20%E5%93%AA%E5%90%92%E7%9B%91%E6%8E%A7%20Agent/Untitled%201.avif)

3. 基本上指定一下服务器地址和密码即可，命令为

    ```bash
    ./nezha-agent -s <Dashboard服务器地址>:<端口> -p <连接密码>
    ```

4. 执行后，没报错，并且在 Web 上能看到信息就可以

    ![](https://hf-image.mitsea.com:8840/blog/posts/2022/10/QNAP%20%E9%83%A8%E7%BD%B2%20%E5%93%AA%E5%90%92%E7%9B%91%E6%8E%A7%20Agent/Untitled%202.avif)

5. 记下自己的执行命令，并把 `./` 替换成绝对路径，比如我的文件放在 `/share/homes/FlintyLemming` 下，那我的执行命令就是

    ```bash
    /share/homes/FlintyLemming/nezha-agent -s <Dashboard服务器地址>:<端口> -p <连接密码>
    ```

## 守护进程

这里是 QTS 比较坑爹的地方。即便内核比隔壁群晖新，已经用上了 5.x，但是它还是用的 SystemV 而不是 SystemD。然后 system、chkconfig 啥的命令通通没有。

![](https://hf-image.mitsea.com:8840/blog/posts/2022/10/QNAP%20%E9%83%A8%E7%BD%B2%20%E5%93%AA%E5%90%92%E7%9B%91%E6%8E%A7%20Agent/Untitled%203.avif)

因为他也没有 rc.d，init.d 我也不好动。所以我打算直接用计划任务土法守护进程。

### 新建脚本

这个我就从之前部署甜糖的脚本抄来用了。找一个地方新建一个 crash_monitor.sh 文件，内容如下

```bash
#!/bin/bash

d=`date '+%F %T'`;
num=`ps fax | grep '/nezha-agent' | egrep -v 'grep|echo|rpm|moni|guard' | wc -l`;
echo $num;
if [ $num -lt 1 ];then
 echo "[$d] nezha-agent is dead...restarting" >> /share/homes/FlintyLemming/log.log ;
 echo "[$d] nezha-agent is dead...restarting";
 /share/homes/FlintyLemming/nezha-agent --password <连接密码> --server <Dashboard服务器地址>:<端口>;
fi
```

echo 那边有一个出 log 的路径，根据实际情况改一下。我把这个脚本还保存在 /share/homes/FlintyLemming 下

### 配置计划任务

计划任务的位置和命令参考 QNAP 官方文档

[Add items to crontab](https://wiki.qnap.com/wiki/Add_items_to_crontab)

先把刚才那个 sh 文档设置权限为可执行

```bash
chmod +x /share/homes/FlintyLemming/crash_monitor.sh
```

然后把上面这个脚本添加到计划任务里

```bash
echo "* * * * * /share/homes/FlintyLemming/crash_monitor.sh" >> /etc/config/crontab
```

重启计划任务

```bash
crontab /etc/config/crontab && /etc/init.d/crond.sh restart
```

### 检查运行

执行下面的命令查看是否有进程

```bash
ps -ef|grep nezha
```

Photo by [Valentin Bolder](https://unsplash.com/es/@vallibo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
  