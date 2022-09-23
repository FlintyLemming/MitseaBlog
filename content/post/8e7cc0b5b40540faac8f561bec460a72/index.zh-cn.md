+++
author = "FlintyLemming"
title = "【归档】ClashR 路由器安装"
slug = "8e7cc0b5b40540faac8f561bec460a72"
date = "2019-11-11"
description = ""
categories = ["HomeLab", "Network"]
tags = ["Clash"]
image = "https://img.mitsea.com/blog/posts/2019/11/ClashR%20%E8%B7%AF%E7%94%B1%E5%99%A8%E5%AE%89%E8%A3%85/title.jpg?x-oss-process=style/ImageCompress"
+++

## 下载内核

[frainzy1477/luci-app-clash](https://github.com/frainzy1477/luci-app-clash/releases)

在这里下载 Clash 的 luci app，你可以把它想象成一个壳子，不包含内核。一般下载 xxx-2_all.ipk 那个文件。

[frainzy1477/clashr](https://github.com/frainzy1477/clashr/releases/tag/v0.16.3)

在这里根据自己的处理器下载内核文件。

## 安装

1. 首先需要将 ipk 文件放到路由器中，这里使用 scp，默认就放在 tmp 文件夹下
    
    ```bash
    scp <ipk文件的绝对路径> <路由器的登陆账号>@<路由器的本地IP地址>:<放入的路径>
    ```
    
    就像下面这样
    
    ```jsx
    scp /Users/flintylemming/Downloads/luci-app-clash_1.2.7-2_all.ipk [root@192.168.1.1](mailto:root@192.168.1.1):/tmp
    ```
    
    之后提示输入密码，输入即可
    
    ![](https://img.mitsea.com/blog/posts/2019/11/ClashR%20%E8%B7%AF%E7%94%B1%E5%99%A8%E5%AE%89%E8%A3%85/1.png?x-oss-process=style/ImageCompress)
    
2. 然后通过 SSH 登陆后安装（当然也可以用网页端的那个终端，注意登陆一定要用 root 登陆，用户名不能置空）
    
    ```bash
    ssh root@<路由器IP地址>
    ```
    
    通过 cd 和 ls 的命令，定位和找到刚才传入的安装包
    
    ```bash
    cd /tmp
    ls
    ```
    
    ![](https://img.mitsea.com/blog/posts/2019/11/ClashR%20%E8%B7%AF%E7%94%B1%E5%99%A8%E5%AE%89%E8%A3%85/2.png?x-oss-process=style/ImageCompress)
    
3. 使用 opkg 命令安装，名字太长可以使用通配符
    
    ```bash
    opkg install luci-app-clash*
    ```
    
    ![](https://img.mitsea.com/blog/posts/2019/11/ClashR%20%E8%B7%AF%E7%94%B1%E5%99%A8%E5%AE%89%E8%A3%85/3.png?x-oss-process=style/ImageCompress)
    
4. 安装完毕后，刷新网页，在服务下就能看到 Clash 了
    
    ![](https://img.mitsea.com/blog/posts/2019/11/ClashR%20%E8%B7%AF%E7%94%B1%E5%99%A8%E5%AE%89%E8%A3%85/4.png?x-oss-process=style/ImageCompress)
    
5. 这只是个壳子，实际上你点开客户端，内核里是没得选的，需要我们放入内核
    
    ![](https://img.mitsea.com/blog/posts/2019/11/ClashR%20%E8%B7%AF%E7%94%B1%E5%99%A8%E5%AE%89%E8%A3%85/5.png?x-oss-process=style/ImageCompress)
    
6. 通过同样的方法安装刚才下载的内核 ipk 文件，之后即可选择内核，至此安装完毕。

> Photo by [NASA](https://unsplash.com/@nasa?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/global?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)