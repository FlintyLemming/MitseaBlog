+++
author = "FlintyLemming"
title = "在需要网络登录验证的环境安装 Linux"
slug = "4d6dbaefb80e4d1692af3b23ab8be33a"
date = "2021-04-22"
categories = ["Linux", "Network"]
tags = ["Ubuntu"]
image = "https://hf-image.mitsea.com:8840/blog/posts/2021/04/%E5%9C%A8%E9%9C%80%E8%A6%81%E7%BD%91%E7%BB%9C%E7%99%BB%E5%BD%95%E9%AA%8C%E8%AF%81%E7%9A%84%E7%8E%AF%E5%A2%83%E5%AE%89%E8%A3%85%20Linux/philip-oroni-0cVTKWWAxV4-unsplash.avif"
+++

首先要了解下 Linux 中的 tty 的概念，具体可以看下面这个解释。简单来说（可能不严谨），你可以类比 macOS 或者 Windows 下的虚拟桌面。

[What does "TTY" stand for?](https://askubuntu.com/questions/481906/what-does-tty-stand-for)

那么启动安装镜像，进入安装界面，就像这样

![](https://hf-image.mitsea.com:8840/blog/posts/2021/04/%E5%9C%A8%E9%9C%80%E8%A6%81%E7%BD%91%E7%BB%9C%E7%99%BB%E5%BD%95%E9%AA%8C%E8%AF%81%E7%9A%84%E7%8E%AF%E5%A2%83%E5%AE%89%E8%A3%85%20Linux/Untitled.avif)

默认应该是 tty1，可以尝试发送 Ctrl+Alt+F1，看看有没有反应，没有的话说明当前就是 tty1。

![](https://hf-image.mitsea.com:8840/blog/posts/2021/04/%E5%9C%A8%E9%9C%80%E8%A6%81%E7%BD%91%E7%BB%9C%E7%99%BB%E5%BD%95%E9%AA%8C%E8%AF%81%E7%9A%84%E7%8E%AF%E5%A2%83%E5%AE%89%E8%A3%85%20Linux/Untitled%201.avif)

要登录公司网络，需要使用 curl 命令发送请求，所以切换到另一个 tty 以输入命令。这边发送 Ctrl+Alt+F2 组合键以切换到 tty2

然后输入命令登录网络即可。我这边用的是深信服的网络登录窗，如何写出这个命令的可以参考这篇文章获取思路。[使用 cli 登陆公司上网验证系统](https://blog.mitsea.com/ddfb7e62396b40c59f74432c862dea69)

![](https://hf-image.mitsea.com:8840/blog/posts/2021/04/%E5%9C%A8%E9%9C%80%E8%A6%81%E7%BD%91%E7%BB%9C%E7%99%BB%E5%BD%95%E9%AA%8C%E8%AF%81%E7%9A%84%E7%8E%AF%E5%A2%83%E5%AE%89%E8%A3%85%20Linux/Untitled%202.avif)

最后，回到 tty1 正常安装系统即可。

> Photo by [Philip Oroni](https://unsplash.com/@philipsfuture?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)