+++
author = "FlintyLemming"
title = "利用群晖自带反代为网络服务配置 https (DSM 7.x)"
slug = "3a0b40567ffe42e999eade2fdaf775b0"
date = "2023-08-17"
description = ""
categories = ["HomeLab", "MineService"]
tags = ["Nginx", "反代"]
image = "https://img.mitsea.com/blog/posts/2023/08/%E5%88%A9%E7%94%A8%E7%BE%A4%E6%99%96%E8%87%AA%E5%B8%A6%E5%8F%8D%E4%BB%A3%E4%B8%BA%E7%BD%91%E7%BB%9C%E6%9C%8D%E5%8A%A1%E9%85%8D%E7%BD%AE%20https%20%28DSM%207.x%29/john-zhou-FrzN1-ENO3Q-unsplash.avif"
+++

群晖无论是商店的套件还是 Docker 都可以自行安装很多网络服务，对于安装的服务应该如何配置 https 呢？在常规 Linux 系统上，一般通过 Nginx、Apache 解决，但群晖内置了基于 Nginx 的图形化反代工具，一切都变得非常简单。

该场景主要是用的 Nginx 的 proxy_pass 功能，它可以让我们通过一个端口、不同的域名就可以访问到本机的多个 Web 服务。效果如下图。

![](https://img.mitsea.com/blog/posts/2023/08/%E5%88%A9%E7%94%A8%E7%BE%A4%E6%99%96%E8%87%AA%E5%B8%A6%E5%8F%8D%E4%BB%A3%E4%B8%BA%E7%BD%91%E7%BB%9C%E6%9C%8D%E5%8A%A1%E9%85%8D%E7%BD%AE%20https%20%28DSM%207.x%29/Untitled.avif)

本篇文章就以 qBittorrent 的 WebUI 为例，介绍如何添加 https

# 前期准备

## 域名

要确保你有一个域名访问服务，并绑定了你公网 IP，如果公网IP是变化的，请使用 DDNS。

## 证书

你要申请一个与访问域名相匹配的证书，并已经配置好在 控制面板 - 安全性 - 证书 下。

![](https://img.mitsea.com/blog/posts/2023/08/%E5%88%A9%E7%94%A8%E7%BE%A4%E6%99%96%E8%87%AA%E5%B8%A6%E5%8F%8D%E4%BB%A3%E4%B8%BA%E7%BD%91%E7%BB%9C%E6%9C%8D%E5%8A%A1%E9%85%8D%E7%BD%AE%20https%20%28DSM%207.x%29/Untitled%201.avif)

## 端口

你还要明确你还想不想通过原来的端口访问服务，如果想，那就要改变原来服务的内网端口。因为来源主机地址的端口不能跟本地端口相同。如果是 Docker 还好办，把映射出来的本地端口改了就行。但如果是套件，控制面板 - 登陆门户 里又没有端口设置，就只能更换访问端口了。

# 配置过程

1. 确认网络服务的本地端口，正如之前提到的，后面是不会通过这个端口访问的。

    ![](https://img.mitsea.com/blog/posts/2023/08/%E5%88%A9%E7%94%A8%E7%BE%A4%E6%99%96%E8%87%AA%E5%B8%A6%E5%8F%8D%E4%BB%A3%E4%B8%BA%E7%BD%91%E7%BB%9C%E6%9C%8D%E5%8A%A1%E9%85%8D%E7%BD%AE%20https%20%28DSM%207.x%29/Untitled%202.avif)

2. 打开 控制面板 - 登陆门户 - 高级 - 反向代理服务器，点 新增

    ![](https://img.mitsea.com/blog/posts/2023/08/%E5%88%A9%E7%94%A8%E7%BE%A4%E6%99%96%E8%87%AA%E5%B8%A6%E5%8F%8D%E4%BB%A3%E4%B8%BA%E7%BD%91%E7%BB%9C%E6%9C%8D%E5%8A%A1%E9%85%8D%E7%BD%AE%20https%20%28DSM%207.x%29/Untitled%203.avif)

3. 这里有一些内容需要填写，一项一项来

    描述 - 随便填一个名字

    来源协议 - HTTPS

    来源主机名 - 看你自己想设置的域名，只要保证你有对应域名的 ssl 证书就行

    来源端口 - 这就是你想访问的那个端口，设置为你自己需要的

    上面这两个设置完后，你在公网访问的地址就是 https://<来源主机名>:<来源端口>

    启动 HSTS - 简单来说就是强制 https 访问，可以开

    启用访问控制 - 可以先不开，有需求自己研究下

    目的地协议 - HTTP

    目的地主机名 - localhost 或者 NAS 本机 IP，如果需要反代其他机器的服务，就填对应机器的 IP

    目的地端口 - 服务的本地端口，qbittorrent 我这里设置的 Web UI IP 是 8844，这里就填 8844

    ![](https://img.mitsea.com/blog/posts/2023/08/%E5%88%A9%E7%94%A8%E7%BE%A4%E6%99%96%E8%87%AA%E5%B8%A6%E5%8F%8D%E4%BB%A3%E4%B8%BA%E7%BD%91%E7%BB%9C%E6%9C%8D%E5%8A%A1%E9%85%8D%E7%BD%AE%20https%20%28DSM%207.x%29/Untitled%204.avif)

4. 路由器 NAT 设置一下，看你来源端口设置的什么，以上面为例。那就设置这台机器 IP 的 4433 对公网 4433
5. 打开 控制面板 - 安全性 - 证书 - 设置，把刚才创建的这一项的证书，选择你前面导入的

    ![](https://img.mitsea.com/blog/posts/2023/08/%E5%88%A9%E7%94%A8%E7%BE%A4%E6%99%96%E8%87%AA%E5%B8%A6%E5%8F%8D%E4%BB%A3%E4%B8%BA%E7%BD%91%E7%BB%9C%E6%9C%8D%E5%8A%A1%E9%85%8D%E7%BD%AE%20https%20%28DSM%207.x%29/Untitled%205.avif)

6. 使用 https://pt.xxx.com:4433 访问即可

> Photo by [John Zhou](https://unsplash.com/@johnnzhou?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/photos/the-night-sky-with-stars-moving-across-the-sky-FrzN1-ENO3Q?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
  