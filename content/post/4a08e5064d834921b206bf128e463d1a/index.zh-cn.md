+++
author = "FlintyLemming"
title = "利用群晖自带反代为网络服务配置 https"
slug = "4a08e5064d834921b206bf128e463d1a"
date = "2020-01-28"
description = ""
categories = ["HomeLab"]
tags = ["Synology"]
image = "https://img.mitsea.com/blog/posts/2020/01/%E5%88%A9%E7%94%A8%E7%BE%A4%E6%99%96%E8%87%AA%E5%B8%A6%E5%8F%8D%E4%BB%A3%E4%B8%BA%E7%BD%91%E7%BB%9C%E6%9C%8D%E5%8A%A1%E9%85%8D%E7%BD%AE%20https/title.avif"
+++

群晖无论是商店的套件还是 Docker 都可以自行安装很多网络服务，有的比如 qbittorrent、Jellyfin 都是内置 https 配置选项的。那对于其他安装的服务应该如何配置 https 呢？在常规 Linux 系统上，一般通过 Nginx、Apache 解决，但群晖内置了基于 Nginx 的图形化反代工具，一切都变得非常简单。

本篇文章就以 Aria2 的 WebUI 为例，介绍如何添加 https

## 前期准备

### 域名

要确保你有一个域名访问服务，并绑定了你公网 IP，如果公网IP是变化的，请使用 DDNS。

### 证书

你要申请一个与访问域名相匹配的证书，并已经配置好在 控制面板 - 安全性 - 证书 下。具体如何获取证书和装载证书，百度教程一大堆。

### 端口

你还要明确你还想不想通过原来的端口访问服务，如果想，那就要改变原来服务的内网端口。因为来源主机地址的端口不能跟本地端口相同。如果是 Docker 还好办，把映射出来的本地端口改了就行。但如果是套件，控制面板 - Synology 应用程序门户 - 应用程序 里又没有端口设置，就只能更换访问端口了。

## 配置过程

1. 确认网络服务的本地端口，正如之前提到的，后面是不会通过这个端口访问的。Docker 这里的话，最好也别用默认端口 8080，因为默认 8080 端口的服务一大堆，这里随便填个比如 8000。至于上面的那个端口是其他作用，与本例无关，请忽略。

    ![](https://img.mitsea.com/blog/posts/2020/01/%E5%88%A9%E7%94%A8%E7%BE%A4%E6%99%96%E8%87%AA%E5%B8%A6%E5%8F%8D%E4%BB%A3%E4%B8%BA%E7%BD%91%E7%BB%9C%E6%9C%8D%E5%8A%A1%E9%85%8D%E7%BD%AE%20https/1.avif)

2. 打开 控制面板 - Synology 应用程序门户 - 反向代理服务器，点 新增

    ![](https://img.mitsea.com/blog/posts/2020/01/%E5%88%A9%E7%94%A8%E7%BE%A4%E6%99%96%E8%87%AA%E5%B8%A6%E5%8F%8D%E4%BB%A3%E4%B8%BA%E7%BD%91%E7%BB%9C%E6%9C%8D%E5%8A%A1%E9%85%8D%E7%BD%AE%20https/2.avif)

3. 这里有一些内容需要填写，一项一项来

    描述 - 随便填一个名字

    来源协议 - HTTPS

    来源主机名 - 留空，默认为 *

    来源端口 - 这就是你想访问的那个端口，设置为你自己需要的

    启动 HSTS - 因为来源主机名没有指明，所以这里开不开没有用，就不开吧

    启用 HTTP/2 - 建议开启，对于带宽比较小的网络环境加载静态页面提升比较大

    启用访问控制 - 可以先不开，有需求自己研究下

    目的地协议 - HTTP

    目的地主机名 - localhost

    目的地端口 - 服务的本地端口，刚才设置的是 8000，这里就填 8000

    ![](https://img.mitsea.com/blog/posts/2020/01/%E5%88%A9%E7%94%A8%E7%BE%A4%E6%99%96%E8%87%AA%E5%B8%A6%E5%8F%8D%E4%BB%A3%E4%B8%BA%E7%BD%91%E7%BB%9C%E6%9C%8D%E5%8A%A1%E9%85%8D%E7%BD%AE%20https/3.avif)

4. 路由器 NAT 设置一下，内网 8857 对公网的 8857
5. 尝试通过 https://<子域名>:8857 访问，成功

    ![](https://img.mitsea.com/blog/posts/2020/01/%E5%88%A9%E7%94%A8%E7%BE%A4%E6%99%96%E8%87%AA%E5%B8%A6%E5%8F%8D%E4%BB%A3%E4%B8%BA%E7%BD%91%E7%BB%9C%E6%9C%8D%E5%8A%A1%E9%85%8D%E7%BD%AE%20https/4.avif)

6. 如果收藏夹存了地址，并且端口没变，记得把原来的地址从 http 改成 https

## 延伸

既然群晖配置反代如此简单，那是不是可以为运行在内网的其他机器上的服务配置呢，你可以自己去尝试一下