+++
author = "FlintyLemming"
title = "借助宽带公网 IP 分发内网内容"
slug = "027af4dd9eba4315a0d3707178080d4c"
date = "2019-10-10"
description = ""
categories = ["HomeLab"]
tags = ["宽带"]
image = "https://img.flinty.moe/blog/posts/2019/10/%E5%80%9F%E5%8A%A9%E5%AE%BD%E5%B8%A6%E5%85%AC%E7%BD%91%20IP%20%E5%88%86%E5%8F%91%E5%86%85%E7%BD%91%E5%86%85%E5%AE%B9/title.avif"
+++

网上有不少将内网内容上到公网的方法，大多都是借助frp。不过随着宽带提供商对于公网IP的放宽，如果有公网IP就可以方便得解决这个问题。

## 获取公网 IP

打电话给宽带提供商，比如我这边的电信，告诉客服需要打开公网IP，一般客服就会给你打开

## 启动项目

启动你需要的项目，我这里随便起一个网页端服务。但是端口选择要注意，常用端口一般会被屏蔽，包括 8080。比如我 8848 端口起了个 qbittorrent 服务

![](https://img.flinty.moe/blog/posts/2019/10/%E5%80%9F%E5%8A%A9%E5%AE%BD%E5%B8%A6%E5%85%AC%E7%BD%91%20IP%20%E5%88%86%E5%8F%91%E5%86%85%E7%BD%91%E5%86%85%E5%AE%B9/1.avif)

8849 端口起了个 jellyfin

![](https://img.flinty.moe/blog/posts/2019/10/%E5%80%9F%E5%8A%A9%E5%AE%BD%E5%B8%A6%E5%85%AC%E7%BD%91%20IP%20%E5%88%86%E5%8F%91%E5%86%85%E7%BD%91%E5%86%85%E5%AE%B9/2.avif)

## 路由器设置

首先你要保证你是在路由器拨号上的网，而不是光猫上拨号，路由器通过DHCP分发网络。可以把网线直接插在光猫上，或者移动设备连接光猫的Wi-Fi（如果有），如果没有网络，则说明确实是通过路由器拨号。我的路由器里有两个可以上公网的方式，分别是 NAT 和 DMZ 功能。如果你的所有服务都在一个设备上，那 NAT 和 DMZ 都可以；如果所有服务分布在内网的不同设备上，就用 NAT 功能。当然，设置前，务必关闭路由器里的防火墙功能。

### NAT 服务

进入路由器的 NAT 服务功能

![](https://img.flinty.moe/blog/posts/2019/10/%E5%80%9F%E5%8A%A9%E5%AE%BD%E5%B8%A6%E5%85%AC%E7%BD%91%20IP%20%E5%88%86%E5%8F%91%E5%86%85%E7%BD%91%E5%86%85%E5%AE%B9/3.avif)

端口映射里，ip选择你启动项目的设备内网IP，内外端口都填你设定的端口

![](https://img.flinty.moe/blog/posts/2019/10/%E5%80%9F%E5%8A%A9%E5%AE%BD%E5%B8%A6%E5%85%AC%E7%BD%91%20IP%20%E5%88%86%E5%8F%91%E5%86%85%E7%BD%91%E5%86%85%E5%AE%B9/4.avif)

端口触发里设置类似

![](https://img.flinty.moe/blog/posts/2019/10/%E5%80%9F%E5%8A%A9%E5%AE%BD%E5%B8%A6%E5%85%AC%E7%BD%91%20IP%20%E5%88%86%E5%8F%91%E5%86%85%E7%BD%91%E5%86%85%E5%AE%B9/5.avif)

如果没什么问题了，公网访问你的IP跟上端口号即可访问。如果还有别的服务和设备，就多添加几条。

### DMZ 主机

填写运行项目的主机内网IP地址即可

![](https://img.flinty.moe/blog/posts/2019/10/%E5%80%9F%E5%8A%A9%E5%AE%BD%E5%B8%A6%E5%85%AC%E7%BD%91%20IP%20%E5%88%86%E5%8F%91%E5%86%85%E7%BD%91%E5%86%85%E5%AE%B9/6.avif)

## 配置 DDNS

1. 在 he 官网注册账号 [https://dns.he.net/](https://dns.he.net/)
2. 注册完毕后，在 [https://dns.he.net](https://dns.he.net/) 这里设置 DNS，他会给你几个 he 的 NS 地址，把这个填到你现在的 NS 服务里解析（需要 NS 服务商支持 NS 记录）

    ![](https://img.flinty.moe/blog/posts/2019/10/%E5%80%9F%E5%8A%A9%E5%AE%BD%E5%B8%A6%E5%85%AC%E7%BD%91%20IP%20%E5%88%86%E5%8F%91%E5%86%85%E7%BD%91%E5%86%85%E5%AE%B9/7.avif)

3. 在 he 的 DNS 设置里添加这个子域名

    ![](https://img.flinty.moe/blog/posts/2019/10/%E5%80%9F%E5%8A%A9%E5%AE%BD%E5%B8%A6%E5%85%AC%E7%BD%91%20IP%20%E5%88%86%E5%8F%91%E5%86%85%E7%BD%91%E5%86%85%E5%AE%B9/8.avif)

4. 点击 Edit Zone 查看记录

    ![](https://img.flinty.moe/blog/posts/2019/10/%E5%80%9F%E5%8A%A9%E5%AE%BD%E5%B8%A6%E5%85%AC%E7%BD%91%20IP%20%E5%88%86%E5%8F%91%E5%86%85%E7%BD%91%E5%86%85%E5%AE%B9/9.avif)

5. 为了避免麻烦，先把默认的几条记录的 TTL 改成五分钟，300秒

    ![](https://img.flinty.moe/blog/posts/2019/10/%E5%80%9F%E5%8A%A9%E5%AE%BD%E5%B8%A6%E5%85%AC%E7%BD%91%20IP%20%E5%88%86%E5%8F%91%E5%86%85%E7%BD%91%E5%86%85%E5%AE%B9/10.avif)

6. 新建一个 A 记录。Name 就填用于 ddns 的域名；IP 地址随便填写一个，这样如果到时还能正确访问自己的公网地址，就说明 ddns 成功生效了；TTL 选择 5 分钟；下面的 Enable entry for dynamic dns 一定要勾选上

    ![](https://img.flinty.moe/blog/posts/2019/10/%E5%80%9F%E5%8A%A9%E5%AE%BD%E5%B8%A6%E5%85%AC%E7%BD%91%20IP%20%E5%88%86%E5%8F%91%E5%86%85%E7%BD%91%E5%86%85%E5%AE%B9/11.avif)

7. 点击这个图标，生成一个 DDNS Key

    ![](https://img.flinty.moe/blog/posts/2019/10/%E5%80%9F%E5%8A%A9%E5%AE%BD%E5%B8%A6%E5%85%AC%E7%BD%91%20IP%20%E5%88%86%E5%8F%91%E5%86%85%E7%BD%91%E5%86%85%E5%AE%B9/12.avif)

8. 可以随机生成一个

    ![](https://img.flinty.moe/blog/posts/2019/10/%E5%80%9F%E5%8A%A9%E5%AE%BD%E5%B8%A6%E5%85%AC%E7%BD%91%20IP%20%E5%88%86%E5%8F%91%E5%86%85%E7%BD%91%E5%86%85%E5%AE%B9/13.avif)

9. 在设备后台运行下面的命令设置 DDNS，注意修改下域名和密码

        curl -4 "https://xxx.xxx.xxx:password@dyn.dns.he.net/nic/update?hostname=xxx.xxx.xxx"

10. 提示 good 即为成功，此时，访问 域名+端口 即可访问页面

    ![](https://img.flinty.moe/blog/posts/2019/10/%E5%80%9F%E5%8A%A9%E5%AE%BD%E5%B8%A6%E5%85%AC%E7%BD%91%20IP%20%E5%88%86%E5%8F%91%E5%86%85%E7%BD%91%E5%86%85%E5%AE%B9/14.avif)

11. 现在需要创建一个计划任务让设备几分钟就执行下这句话。但由于群晖不能直接通过 crontab 命令创建任务。首先需要切换到 root 账户，然后编辑计划任务的文件

        root@DS918Plus:~# vim /etc/crontab

12. 新建一条计划任务

        #minute    hour    mday    month   wday    who    command
        5    *    *    *    *    root curl -4 "[https://ddns.flinty.moe:4CBbvDalUOfxvXr0@dyn.dns.he.net/nic/update?hostname=ddns.flinty.moe](https://ddns.flinty.moe:4CBbvDalUOfxvXr0@dyn.dns.he.net/nic/update?hostname=ddns.flinty.moe)"

13. 修改后，重启服务

        root@DS918Plus:~# synoservice -restart crond

14. 此时，在外网访问 ddns 域名+端口号即可访问服务页面。