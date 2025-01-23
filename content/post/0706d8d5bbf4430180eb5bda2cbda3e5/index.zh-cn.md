+++
author = "FlintyLemming"
title = "【归档】Mojave 的 Safari 如何安装没有证书的扩展"
slug = "0706d8d5bbf4430180eb5bda2cbda3e5"
date = "2020-01-19"
description = ""
categories = ["Apple"]
tags = ["Mojave", "Safari"]
image = "https://image.mitsea.com/blog/posts/2020/01/Mojave%20%E7%9A%84%20Safari%20%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%85%E6%B2%A1%E6%9C%89%E8%AF%81%E4%B9%A6%E7%9A%84%E6%89%A9%E5%B1%95/title.avif"
+++

在 macOS Mojave 中，Safari 不允许安装没有签名的扩展了，但仍然有方法安装。

![](https://image.mitsea.com/blog/posts/2020/01/Mojave%20%E7%9A%84%20Safari%20%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%85%E6%B2%A1%E6%9C%89%E8%AF%81%E4%B9%A6%E7%9A%84%E6%89%A9%E5%B1%95/1.avif)

贴吧老哥跟我说了一个方法，具体的链接贴在[这里](https://tieba.baidu.com/p/5822937990#121303346729)，来源是V2EX，但非常抱歉我没有地址。

## 具体步骤

1. 解压下载的 .safariextz 文件，注意这里不能通过改后缀为 .zip 然后解压，需要通过第三方工具，比如 The Unarchiver

    ![](https://image.mitsea.com/blog/posts/2020/01/Mojave%20%E7%9A%84%20Safari%20%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%85%E6%B2%A1%E6%9C%89%E8%AF%81%E4%B9%A6%E7%9A%84%E6%89%A9%E5%B1%95/2.avif)

2. 在 Safari 的偏好设置里打开“开发”菜单

    ![](https://image.mitsea.com/blog/posts/2020/01/Mojave%20%E7%9A%84%20Safari%20%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%85%E6%B2%A1%E6%9C%89%E8%AF%81%E4%B9%A6%E7%9A%84%E6%89%A9%E5%B1%95/3.avif)

3. 点击 开发-显示扩展构建器
4. 点击左下角的加号，选择 添加扩展，然后选择刚才解压的文件夹

    ![](https://image.mitsea.com/blog/posts/2020/01/Mojave%20%E7%9A%84%20Safari%20%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%85%E6%B2%A1%E6%9C%89%E8%AF%81%E4%B9%A6%E7%9A%84%E6%89%A9%E5%B1%95/4.avif)

5. 看到加载的扩展后，点击右上角的运行，输入密码后即可安装扩展
