+++
author = "FlintyLemming"
title = "Windows 10 连接 IPSec 未加密的 L2TP VPN"
slug = "683396e4936841a8b82181d9fd8ccf42"
date = "2020-04-04"
description = ""
categories = ["Windows"]
tags = ["Windows", "VPN"]
image = "https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2020/04/Windows%2010%20%E8%BF%9E%E6%8E%A5%20IPSec%20%E6%9C%AA%E5%8A%A0%E5%AF%86%E7%9A%84%20L2TP%20VPN/title.avif"
+++

配置 L2TP VPN 的时候，如果设置了 预共享秘钥 但是 IPSec 未加密（群晖的 VPN Server 套件就是），在使用 Windows 10 的时候会连不上。显示如下错误

无法建立计算机与 VPN 服务器之间的网络连接，因为远程服务器未响应。这可能是因为未将计算机与远程服务器之间的某种网络设备（如防火墙、NAT、路由器等）配置为允许 VPN 连接。请与管理员或服务提供商联系以确定哪种设备可能产生此问题。

或者是

L2TP 连接尝试失败，因为安全层初始化与远程计算机的协商时遇到一个处理错误。

下面提供两种解决方法

## 使用第三方 L2TP VPN 连接工具

这里推荐我在公司连接客户 VPN 时使用的软件，SonicWALL Global VPN Client

![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2020/04/Windows%2010%20%E8%BF%9E%E6%8E%A5%20IPSec%20%E6%9C%AA%E5%8A%A0%E5%AF%86%E7%9A%84%20L2TP%20VPN/1.avif)

这个软件的话，直接按照向导创建一个新的 VPN 连接即可

![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2020/04/Windows%2010%20%E8%BF%9E%E6%8E%A5%20IPSec%20%E6%9C%AA%E5%8A%A0%E5%AF%86%E7%9A%84%20L2TP%20VPN/2.avif)

但这个软件有个很捞的地方，就是用户名和密码保存不了，每次连接都要输入一次。所以我更加推荐下面这种方法。

## 修改注册表

首先如果是公司的电脑并且加了域的情况下，你要确认你能不能动注册表。动不了的话那你还是老老实实用上面那个软件。

1. 打开注册表编辑器，定位到 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\RasMan\Parameters
2. 然后新增一个名为 ProhibitIPSec ，类型为 DWORD （32位）的键，值为 0

    ![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2020/04/Windows%2010%20%E8%BF%9E%E6%8E%A5%20IPSec%20%E6%9C%AA%E5%8A%A0%E5%AF%86%E7%9A%84%20L2TP%20VPN/3.avif)

3. 接着定位到 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\PolicyAgent
4. 然后新增一个名为 AssumeUDPEncapsulationContextOnSendRule ，类型为 DWORD （32位）的键，值为 2

    ![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2020/04/Windows%2010%20%E8%BF%9E%E6%8E%A5%20IPSec%20%E6%9C%AA%E5%8A%A0%E5%AF%86%E7%9A%84%20L2TP%20VPN/4.avif)

5. 然后将服务 IPsec Policy Agent（PolicyAgent）改为手动

    ![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2020/04/Windows%2010%20%E8%BF%9E%E6%8E%A5%20IPSec%20%E6%9C%AA%E5%8A%A0%E5%AF%86%E7%9A%84%20L2TP%20VPN/5.avif)

6. 然后就能正常连接了

## 参考

[https://www.heylc.com/code/4.html](https://www.heylc.com/code/4.html)