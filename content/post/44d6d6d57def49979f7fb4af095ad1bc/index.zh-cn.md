+++
author = "FlintyLemming"
title = "家有公网IP 利用frp远程无公网IP的电脑"
slug = "44d6d6d57def49979f7fb4af095ad1bc"
date = "2020-01-11"
description = ""
categories = ["HomeLab"]
tags = ["宽带"]
image = "https://img.mitsea.com/blog/posts/2020/01/%E5%AE%B6%E6%9C%89%E5%85%AC%E7%BD%91IP%20%E5%88%A9%E7%94%A8frp%E8%BF%9C%E7%A8%8B%E6%97%A0%E5%85%AC%E7%BD%91IP%E7%9A%84%E7%94%B5%E8%84%91/title.jpg?x-oss-process=style/ImageCompress"
+++

这里我原来真是蠢到家了，明明自己的宽带有公网 IP，还去找机房做 frp 服务器，明明本地就能解决。下面说下步骤吧

## 在家中的网络搭建服务端

这边我建议是弄个常开的设备搭建服务，我这里是用的 NAS 里的 docker

1. 搜索一下可以看到很多关于 frp 服务的镜像

    ![](https://img.mitsea.com/blog/posts/2020/01/%E5%AE%B6%E6%9C%89%E5%85%AC%E7%BD%91IP%20%E5%88%A9%E7%94%A8frp%E8%BF%9C%E7%A8%8B%E6%97%A0%E5%85%AC%E7%BD%91IP%E7%9A%84%E7%94%B5%E8%84%91/1.png?x-oss-process=style/ImageCompress)

    注意不要下载客户端，也就是那个 frpc，要下载服务端，frps，server

2. 下载完后配置镜像，这里可能还是有人不会配置下载好的镜像哈，其实 docker 使用主要特点就是方便，所以并不困难。配置docker主要就是 目录、端口和环境变量。前两个讲究的是一一对应，比如 docker 内的目录对应的是外面的哪个目录，端口同理；而环境变量主要是要查询说明文档看看有什么变量可以修改的。下面来看下如何配置。
3. 首先先点击刚才查到的镜像右边的按钮，在 docker hub 中找到这个镜像说明

    [Docker Hub](https://registry.hub.docker.com/r/cloverzrg/frps-docker)

4. 看下运行需要什么东西，首先看下运行命令

        docker run -d --name frp-server -p <HOST_PORT>:<CONTAINER_PORT> -v <ABSOLUTE_PATH>/conf:/conf --restart=always cloverzrg/frps-docker

    然后可以看到下面还给了一个配置文件的示例，运行命令里也需要这个配置文件，路径为 /conf。

        # vi /root/conf/frps.ini

    那基本就可以确定了，这个程序基于这个配置文件运行。

5. 然后我们要取得这个配置文件，就需要把这个路径从 docker 里面的路径，映射到外部我们方便编辑的路径

    ![](https://img.mitsea.com/blog/posts/2020/01/%E5%AE%B6%E6%9C%89%E5%85%AC%E7%BD%91IP%20%E5%88%A9%E7%94%A8frp%E8%BF%9C%E7%A8%8B%E6%97%A0%E5%85%AC%E7%BD%91IP%E7%9A%84%E7%94%B5%E8%84%91/2.png?x-oss-process=style/ImageCompress)

    左边就是实际存在的路径，这个随便写一个地方，好管理就行。右边就是 docker 里面的路径。也就是我们能在左边的路径里，看到右边路径里的文件，这样就很方便的编辑 docker 里面的文件了。

6. 继续看文档，文档一定要仔细看，发现配置文件里用到一些端口，最基本的就是这个

        [common]
        bind_port = 7700

    看起来我们要设置一个frp服务端口，他默认是 7700，可以改可以不改。因为之前说了，这是 docker 里面的端口，要映射出来才能用。如果需要自定义端口，只需要设置映射出来的端口就行。那我这里想自定义端口为 8851，就在 端口设置 里这样填写。然后，还要填写个你在客户端上可能用到的服务的端口，比如我要用 rdp ，转发端口是 8847。为什么要加这一条，后面会提到。

    ![](https://img.mitsea.com/blog/posts/2020/01/%E5%AE%B6%E6%9C%89%E5%85%AC%E7%BD%91IP%20%E5%88%A9%E7%94%A8frp%E8%BF%9C%E7%A8%8B%E6%97%A0%E5%85%AC%E7%BD%91IP%E7%9A%84%E7%94%B5%E8%84%91/3.png?x-oss-process=style/ImageCompress)

7. 保存完毕后先别着急运行，先编辑下配置文件。进入到刚才映射出来的目录，发现甚至连预设文件都没给一个

    ![](https://img.mitsea.com/blog/posts/2020/01/%E5%AE%B6%E6%9C%89%E5%85%AC%E7%BD%91IP%20%E5%88%A9%E7%94%A8frp%E8%BF%9C%E7%A8%8B%E6%97%A0%E5%85%AC%E7%BD%91IP%E7%9A%84%E7%94%B5%E8%84%91/4.png?x-oss-process=style/ImageCompress)

8. 创建一个 frps.ini 文件，填写如下内容。因为这次功能要求不多，只要能转发就行。

    ![](https://img.mitsea.com/blog/posts/2020/01/%E5%AE%B6%E6%9C%89%E5%85%AC%E7%BD%91IP%20%E5%88%A9%E7%94%A8frp%E8%BF%9C%E7%A8%8B%E6%97%A0%E5%85%AC%E7%BD%91IP%E7%9A%84%E7%94%B5%E8%84%91/5.png?x-oss-process=style/ImageCompress)

    你甚至连 token 都可以不写，不过我觉得不太安全

9. 保存后，尝试运行镜像，看下 log，哎可以了

    ![](https://img.mitsea.com/blog/posts/2020/01/%E5%AE%B6%E6%9C%89%E5%85%AC%E7%BD%91IP%20%E5%88%A9%E7%94%A8frp%E8%BF%9C%E7%A8%8B%E6%97%A0%E5%85%AC%E7%BD%91IP%E7%9A%84%E7%94%B5%E8%84%91/6.png?x-oss-process=style/ImageCompress)

## 在路由器设置 NAT

要想使得无公网IP的电脑能连上你的 frp 服务，你需要映射你的服务端口到公网，这个用过公网 IP 服务的应该都熟悉了。我这边在路由器上就是这么设置的。

![](https://img.mitsea.com/blog/posts/2020/01/%E5%AE%B6%E6%9C%89%E5%85%AC%E7%BD%91IP%20%E5%88%A9%E7%94%A8frp%E8%BF%9C%E7%A8%8B%E6%97%A0%E5%85%AC%E7%BD%91IP%E7%9A%84%E7%94%B5%E8%84%91/7.png?x-oss-process=style/ImageCompress)

## 在无公网IP的电脑配置客户端

1. 从 frp 的 GitHub [release 页面](https://github.com/fatedier/frp/releases)下载 frp，无公网IP的电脑是 Windows，那就下载 frp_0.31.1_windows_amd64.zip
2. 下载完后解压，然后我们用的是客户端，那就先编辑 frpc.ini 文件
3. 基本也就用个远程访问，那就按照下面配置

        [common]
        server_addr = <你家公网IP地址或者ddns域名>
        server_port = <路由器中设置 NAT 转发出来的端口>
        token = <刚才设置的密码>
        
        [rdp]
        type = tcp
        local_ip = 0.0.0.0
        local_port = 3389 #这是 rdp 的默认端口
        remote_port = 8847 #相当于 NAT，也就是你实际会访问的端口

    注意这个 remote_port 就是为什么我刚才 docker 配置时说要再加一条

4. 然后 cd 到 frp 的目录，输入  frpc.exe 运行

    ![](https://img.mitsea.com/blog/posts/2020/01/%E5%AE%B6%E6%9C%89%E5%85%AC%E7%BD%91IP%20%E5%88%A9%E7%94%A8frp%E8%BF%9C%E7%A8%8B%E6%97%A0%E5%85%AC%E7%BD%91IP%E7%9A%84%E7%94%B5%E8%84%91/8.jpeg?x-oss-process=style/ImageCompress)

    可以看到运行成功

## 在家中电脑使用

1. 打开远程桌面链接，搜索框中搜索 rdp 即可找到。

    计算机 填写 <服务端本地地址>:<刚才设置的 remote_port 端口>

    ![](https://img.mitsea.com/blog/posts/2020/01/%E5%AE%B6%E6%9C%89%E5%85%AC%E7%BD%91IP%20%E5%88%A9%E7%94%A8frp%E8%BF%9C%E7%A8%8B%E6%97%A0%E5%85%AC%E7%BD%91IP%E7%9A%84%E7%94%B5%E8%84%91/9.png?x-oss-process=style/ImageCompress)

2. 成功远程

    ![](https://img.mitsea.com/blog/posts/2020/01/%E5%AE%B6%E6%9C%89%E5%85%AC%E7%BD%91IP%20%E5%88%A9%E7%94%A8frp%E8%BF%9C%E7%A8%8B%E6%97%A0%E5%85%AC%E7%BD%91IP%E7%9A%84%E7%94%B5%E8%84%91/10.png?x-oss-process=style/ImageCompress)

## 在外使用

1. 在家里路由器上配置 NAT，转发那个 remote_port 端口

    ![](https://img.mitsea.com/blog/posts/2020/01/%E5%AE%B6%E6%9C%89%E5%85%AC%E7%BD%91IP%20%E5%88%A9%E7%94%A8frp%E8%BF%9C%E7%A8%8B%E6%97%A0%E5%85%AC%E7%BD%91IP%E7%9A%84%E7%94%B5%E8%84%91/11.png?x-oss-process=style/ImageCompress)

2. 然后就可以用 <公网IP地址或DDNS域名>:<remote_port 端口> 访问了