+++
author = "FlintyLemming"
title = "集中监测多台设备的硬盘 SMART 信息"
slug = "4b50404f8a384310a45490ae6568f257"
date = "2023-06-01"
description = "数据党狂喜"
categories = ["HomeLab", "MineService"]
tags = ["硬盘", "NAS"]
image = "https://assets.mitsea.cn/blog/posts/2023/06/%E9%9B%86%E4%B8%AD%E7%9B%91%E6%B5%8B%E5%A4%9A%E5%8F%B0%E8%AE%BE%E5%A4%87%E7%9A%84%E7%A1%AC%E7%9B%98%20SMART%20%E4%BF%A1%E6%81%AF/jaredd-craig-9kTHBEaFmhU-unsplash.avif"
+++

[Demo Site](https://disks.mitsea.com/web/dashboard)

只说思路，具体操作可以自行 GPT

## 概要

用的工具是下面这个

[https://github.com/AnalogJ/scrutiny](https://github.com/AnalogJ/scrutiny)

根据[这个容器编排文档](https://github.com/AnalogJ/scrutiny/blob/master/docker/example.hubspoke.docker-compose.yml)可以看到它分为 web、数据库、collector。原理就是把 web+数据库 部署在一台服务器上以供访问，collector 分别部署到每个机器上，然后通过 web 的 API 传回硬盘信息。

## 注意点

### 执行 collector

collector 容器部署完后创建了一个计划任务，但不会立即执行一次，需要手动执行容器里面的一个二进制程序

```bash
docker exec scrutiny-collector /opt/scrutiny/bin/scrutiny-collector-metrics run
```

### 群晖的 /dev

使用设备树的群晖机型，SATA 硬盘在 /dev 下面叫 sata1、sata2… 不是 sda、sdb…，collector 是不会主动去扫描这些设备的，所以除了在创建容器时映射设备，还要写一个 collector 的 config 文件

```yaml
version: 1
devices:
  - device: /dev/sata1
    type: 'sat'
  - device: /dev/sata2
    type: 'sat'
  - device: /dev/sata3
    type: 'sat'
  - device: /dev/sata4
    type: 'sat'
```

nvme 设备不用写，因为即便没有配置文件，collector 也会读 nvme 设备，然后你创建容器的时候映射一下文件。collector 在启动时会先读这个配置文件

```bash
-v /xxx/collector.yaml:/opt/scrutiny/config/collector.yaml
```

如果硬盘名字就叫 sda、sdb…就可以不用准备这个文件

### 设备分组

我的页面里你可以看到每个机器都是有名字分组的，这个在创建容器时设置一个环境变量即可

```bash
-e COLLECTOR_HOST_ID="HKT Proxmox”
```

### 保护站点

站点实际上也是个 API 后端，collector 都是通过 web 的 API 添加硬盘。网页上也是可以删除硬盘。为了阻止他人恶意往你的站点添加硬盘或者删除硬盘，可以在 nginx 配置中把这些都拒绝掉

```bash
location /api/devices/register {
    deny all;
}

if ($request_method = DELETE) {
    return 444;
}
```

为了不影响自己使用，可以再创建个自用的 vhost，然后在 nginx 上加上访问密码，这样你 collector 容器里的环境变量可以设置为这样子的形式

```bash
-e COLLECTOR_API_ENDPOINT=https://xxx:xxx@xxx.xxx.xxx
```

> Photo by [Jaredd Craig](https://unsplash.com/@jareddc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
