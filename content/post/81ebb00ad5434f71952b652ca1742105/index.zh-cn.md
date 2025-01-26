+++
author = "FlintyLemming"
title = "Windows Server 2019 连不上 smb 的问题"
slug = "81ebb00ad5434f71952b652ca1742105"
date = "2020-11-09"
categories = ["Linux", "Windows"]
tags = ["smb"]
image = "https://hf-image.mitsea.com:8840/blog/posts/2020/10/Windows%20Server%202019%20%E8%BF%9E%E4%B8%8D%E4%B8%8A%20smb%20%E7%9A%84%E9%97%AE%E9%A2%98/max-kukurudziak-eQA03CVWOT0-unsplash.avif"
+++

不知道为啥，我用 Windows Server 2019 连群晖的 smb 总是连不上，会提示下面这个

![](https://hf-image.mitsea.com:8840/blog/posts/2020/10/Windows%20Server%202019%20%E8%BF%9E%E4%B8%8D%E4%B8%8A%20smb%20%E7%9A%84%E9%97%AE%E9%A2%98/2020-10-27_2.26.41.avif)

但是 ping 能正常 ping 到机器，网页也能访问。所以猜测可能是验证出了问题，没有弹出下面这个框子。

![](https://hf-image.mitsea.com:8840/blog/posts/2020/10/Windows%20Server%202019%20%E8%BF%9E%E4%B8%8D%E4%B8%8A%20smb%20%E7%9A%84%E9%97%AE%E9%A2%98/2020-10-27_2.38.23.avif)

猜到原因的话就比较好办了，只要手动添加一个 Credential 就可以

在 Control Panel - User Accounts - Credential Manager 页面下，点击 Windows Credentials - Add a Windows credential

![](https://hf-image.mitsea.com:8840/blog/posts/2020/10/Windows%20Server%202019%20%E8%BF%9E%E4%B8%8D%E4%B8%8A%20smb%20%E7%9A%84%E9%97%AE%E9%A2%98/2020-10-27_2.39.12.avif)

在下面的窗口中手动添加凭据就可以

![](https://hf-image.mitsea.com:8840/blog/posts/2020/10/Windows%20Server%202019%20%E8%BF%9E%E4%B8%8D%E4%B8%8A%20smb%20%E7%9A%84%E9%97%AE%E9%A2%98/2020-10-27_3.15.39.avif)

不知道为什么那个输入账号密码的框子没有弹出来，可能哪里设置有问题

> Photo by [Max Kukurudziak](https://unsplash.com/@maxkuk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
