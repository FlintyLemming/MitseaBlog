+++
author = "FlintyLemming"
title = "自建 Tailscale DERP"
slug = "c048b27771374b6588b5ff08563e1f3c"
date = "2022-09-17"
description = ""
categories = ["Network", "Linux"]
tags = ["Tailscale"]
image = "https://img.mitsea.com/blog/posts/2022/09/%E8%87%AA%E5%BB%BA%20Tailscale%20DERP/title.jpg?x-oss-process=style/ImageCompress"
+++

## 配置 Go 环境

以下参考 

[How To Install Go on Ubuntu 20.04 | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-install-go-on-ubuntu-20-04)

1. 在 Go 官网查找并下载最新版
    
    [Downloads - The Go Programming Language](https://go.dev/dl/)
    
2. 解压并放到 `/usr/local` 文件夹中
    
    ```bash
    sudo tar -C /usr/local -xvf go1.19.1.linux-amd64.tar.gz
    ```
    
3. 配置 shell 环境
    
    编辑 `.profile` 文件
    
    ```bash
    sudo nano ~/.profile
    ```
    
    在最后一行加上
    
    ```bash
    export PATH=$PATH:/usr/local/go/bin
    ```
    
    刷新设置
    
    ```bash
    source ~/.profile
    ```
    
    执行 `go version` 命令，能看到版本就行
    

## 安装 DERP

执行下面的命令安装

```bash
go install tailscale.com/cmd/derper@main
```

## 准备 DERP 所需内容（使用 443 端口）

### 域名

准备一个子域名，解析到当前服务器

### SSL 证书

准备域名对应的 SSL 证书，注意名字必须是 <域名>.crt 和 <域名>.key，放在某个文件夹里

## 启动 DERP

```bash
/root/go/bin/derper -hostname <域名> -certdir <证书存放路径> -certmode manual -verify-clients
```

保活的话，可以把这个做成服务或者直接丢到一个 screen 里跑

## 防止被滥用

默认情况下，只要别人知道你这个域名，就可以使用你的 DERP 服务器。

要想不被滥用也很简单，因为我们之前已经在启动的时候添加了 `-verify-clients` flag，所以现在只需要将这台 DERP 服务器也加入到你的 Tailscale 里就行。

> Photo by [Martin Sanchez](https://unsplash.com/@martinsanchez?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/global?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
  