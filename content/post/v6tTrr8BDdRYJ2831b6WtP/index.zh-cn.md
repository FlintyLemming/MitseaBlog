+++
author = "FlintyLemming"
title = "在使用微软账号初始化的设备上启用远程桌面"
slug = "v6tTrr8BDdRYJ2831b6WtP"
date = "2024-12-04"
description = ""
categories = ["Microsoft", "Windows"]
tags = ["远程桌面"]
image = "https://blog-img.mitsea.com/images/blog/posts/2024/12/%E5%9C%A8%E4%BD%BF%E7%94%A8%E5%BE%AE%E8%BD%AF%E8%B4%A6%E5%8F%B7%E5%88%9D%E5%A7%8B%E5%8C%96%E7%9A%84%E8%AE%BE%E5%A4%87%E4%B8%8A%E5%90%AF%E7%94%A8%E8%BF%9C%E7%A8%8B%E6%A1%8C%E9%9D%A2/pawel-czerwinski-wSWJo7cTZNA-unsplash.avif"
+++

## 背景

如果在初始化电脑的过程中直接使用微软账号，默认情况下是不能使用账号密码使用远程桌面登陆到这台计算机的。特别是在 Windows 11 新版本中默认不让你配置本地账号，导致很多人在初始化电脑的时候实际上使用的是微软账号登陆的，然后设置的是一个 PIN，并不是传统意义上的账号密码

![](https://blog-img.mitsea.com/images/blog/posts/2024/12/%E5%9C%A8%E4%BD%BF%E7%94%A8%E5%BE%AE%E8%BD%AF%E8%B4%A6%E5%8F%B7%E5%88%9D%E5%A7%8B%E5%8C%96%E7%9A%84%E8%AE%BE%E5%A4%87%E4%B8%8A%E5%90%AF%E7%94%A8%E8%BF%9C%E7%A8%8B%E6%A1%8C%E9%9D%A2/CleanShot%202024-12-03%20at%2017.34.14@2x_DIjFhetbv5.avif)

这个 PIN 是不可以拿来做为远程桌面的账号密码的

![](https://blog-img.mitsea.com/images/blog/posts/2024/12/%E5%9C%A8%E4%BD%BF%E7%94%A8%E5%BE%AE%E8%BD%AF%E8%B4%A6%E5%8F%B7%E5%88%9D%E5%A7%8B%E5%8C%96%E7%9A%84%E8%AE%BE%E5%A4%87%E4%B8%8A%E5%90%AF%E7%94%A8%E8%BF%9C%E7%A8%8B%E6%A1%8C%E9%9D%A2/CleanShot%202024-12-04%20at%2009.26.08@2x_Rb2n-89j8V.avif)

## 解决方法

### 方法一：允许微软账号密码登陆

1. 设置里打开 账户 - 登录选项

   ![](https://blog-img.mitsea.com/images/blog/posts/2024/12/%E5%9C%A8%E4%BD%BF%E7%94%A8%E5%BE%AE%E8%BD%AF%E8%B4%A6%E5%8F%B7%E5%88%9D%E5%A7%8B%E5%8C%96%E7%9A%84%E8%AE%BE%E5%A4%87%E4%B8%8A%E5%90%AF%E7%94%A8%E8%BF%9C%E7%A8%8B%E6%A1%8C%E9%9D%A2/CleanShot%202024-12-04%20at%2010.17.40@2x_KWovCCl3HS.avif)
2. 关闭下方的“为了提高安全性，仅允许对此设备上的 Microsoft 账户使用 Windows Hello 登陆”

   ![](https://blog-img.mitsea.com/images/blog/posts/2024/12/%E5%9C%A8%E4%BD%BF%E7%94%A8%E5%BE%AE%E8%BD%AF%E8%B4%A6%E5%8F%B7%E5%88%9D%E5%A7%8B%E5%8C%96%E7%9A%84%E8%AE%BE%E5%A4%87%E4%B8%8A%E5%90%AF%E7%94%A8%E8%BF%9C%E7%A8%8B%E6%A1%8C%E9%9D%A2/CleanShot%202024-12-04%20at%2010.39.56@2x_Zrskt7Lqz9.avif)
3. 返回上一级，再重新进入这个页面，就可以看到密码选项了，这个是你微软账户登陆的密码

   ![](https://blog-img.mitsea.com/images/blog/posts/2024/12/%E5%9C%A8%E4%BD%BF%E7%94%A8%E5%BE%AE%E8%BD%AF%E8%B4%A6%E5%8F%B7%E5%88%9D%E5%A7%8B%E5%8C%96%E7%9A%84%E8%AE%BE%E5%A4%87%E4%B8%8A%E5%90%AF%E7%94%A8%E8%BF%9C%E7%A8%8B%E6%A1%8C%E9%9D%A2/CleanShot%202024-12-04%20at%2010.40.54@2x_RK9ib-6P42.avif)
4. 注销登陆，选择登陆选项，点击右面的按钮使用密码登陆，然后输入你的微软账号的密码登陆一次

   ![](https://blog-img.mitsea.com/images/blog/posts/2024/12/%E5%9C%A8%E4%BD%BF%E7%94%A8%E5%BE%AE%E8%BD%AF%E8%B4%A6%E5%8F%B7%E5%88%9D%E5%A7%8B%E5%8C%96%E7%9A%84%E8%AE%BE%E5%A4%87%E4%B8%8A%E5%90%AF%E7%94%A8%E8%BF%9C%E7%A8%8B%E6%A1%8C%E9%9D%A2/CleanShot%202024-12-04%20at%2010.42.11@2x_rPW3joEgFM.avif)
5. 再次确保远程桌面已启用

   ![](https://blog-img.mitsea.com/images/blog/posts/2024/12/%E5%9C%A8%E4%BD%BF%E7%94%A8%E5%BE%AE%E8%BD%AF%E8%B4%A6%E5%8F%B7%E5%88%9D%E5%A7%8B%E5%8C%96%E7%9A%84%E8%AE%BE%E5%A4%87%E4%B8%8A%E5%90%AF%E7%94%A8%E8%BF%9C%E7%A8%8B%E6%A1%8C%E9%9D%A2/CleanShot%202024-12-04%20at%2010.20.53@2x_z6FcfYa0m7.avif)

6. 打开任务管理器，切换到网卡，再次确认当前 IP 地址

   ![](https://blog-img.mitsea.com/images/blog/posts/2024/12/%E5%9C%A8%E4%BD%BF%E7%94%A8%E5%BE%AE%E8%BD%AF%E8%B4%A6%E5%8F%B7%E5%88%9D%E5%A7%8B%E5%8C%96%E7%9A%84%E8%AE%BE%E5%A4%87%E4%B8%8A%E5%90%AF%E7%94%A8%E8%BF%9C%E7%A8%8B%E6%A1%8C%E9%9D%A2/CleanShot%202024-12-04%20at%2010.23.48@2x_I9yleKsgRM.avif)
7. 远程桌面客户端直接使用微软账号和密码登陆

   ![](https://blog-img.mitsea.com/images/blog/posts/2024/12/%E5%9C%A8%E4%BD%BF%E7%94%A8%E5%BE%AE%E8%BD%AF%E8%B4%A6%E5%8F%B7%E5%88%9D%E5%A7%8B%E5%8C%96%E7%9A%84%E8%AE%BE%E5%A4%87%E4%B8%8A%E5%90%AF%E7%94%A8%E8%BF%9C%E7%A8%8B%E6%A1%8C%E9%9D%A2/CleanShot%202024-12-04%20at%2010.23.04@2x_yxNdAKxSdx.avif)

   对于一些古老的客户端可能需要手动添加域，即用户名输入 `MicrosoftAccount\muchenran@live.com`&#x20;

### 方法二：切换为本地账号（适用于所有场合）

1. 账号管理页面里点击 个人信息

   ![](https://blog-img.mitsea.com/images/blog/posts/2024/12/%E5%9C%A8%E4%BD%BF%E7%94%A8%E5%BE%AE%E8%BD%AF%E8%B4%A6%E5%8F%B7%E5%88%9D%E5%A7%8B%E5%8C%96%E7%9A%84%E8%AE%BE%E5%A4%87%E4%B8%8A%E5%90%AF%E7%94%A8%E8%BF%9C%E7%A8%8B%E6%A1%8C%E9%9D%A2/CleanShot%202024-12-04%20at%2009.32.48@2x_M81G3GTST5.avif)
2. 点击 改用本地账户

   ![](https://blog-img.mitsea.com/images/blog/posts/2024/12/%E5%9C%A8%E4%BD%BF%E7%94%A8%E5%BE%AE%E8%BD%AF%E8%B4%A6%E5%8F%B7%E5%88%9D%E5%A7%8B%E5%8C%96%E7%9A%84%E8%AE%BE%E5%A4%87%E4%B8%8A%E5%90%AF%E7%94%A8%E8%BF%9C%E7%A8%8B%E6%A1%8C%E9%9D%A2/CleanShot%202024-12-04%20at%2009.37.28@2x_dwIjJ8dUsr.avif)
3. 然后这里就会让你设置账号密码，注意这里账号名是可以修改的，他会帮你修改用户文件夹名称，你的文件都不会丢失

   ![](https://blog-img.mitsea.com/images/blog/posts/2024/12/%E5%9C%A8%E4%BD%BF%E7%94%A8%E5%BE%AE%E8%BD%AF%E8%B4%A6%E5%8F%B7%E5%88%9D%E5%A7%8B%E5%8C%96%E7%9A%84%E8%AE%BE%E5%A4%87%E4%B8%8A%E5%90%AF%E7%94%A8%E8%BF%9C%E7%A8%8B%E6%A1%8C%E9%9D%A2/CleanShot%202024-12-04%20at%2009.57.10@2x_U5PT0SRnnq.avif)

### 方法三：添加微软账号密码（适用于无密码账户）

微软提供了无密码账户的选项，启用后，你的微软账号密码就会被删除

![](https://blog-img.mitsea.com/images/blog/posts/2024/12/%E5%9C%A8%E4%BD%BF%E7%94%A8%E5%BE%AE%E8%BD%AF%E8%B4%A6%E5%8F%B7%E5%88%9D%E5%A7%8B%E5%8C%96%E7%9A%84%E8%AE%BE%E5%A4%87%E4%B8%8A%E5%90%AF%E7%94%A8%E8%BF%9C%E7%A8%8B%E6%A1%8C%E9%9D%A2/CleanShot%202024-12-04%20at%2009.23.45@2x_12SdC5mnmz.avif)

使用无密码账号登陆后，登陆选项里也看不到密码选项

![](https://blog-img.mitsea.com/images/blog/posts/2024/12/%E5%9C%A8%E4%BD%BF%E7%94%A8%E5%BE%AE%E8%BD%AF%E8%B4%A6%E5%8F%B7%E5%88%9D%E5%A7%8B%E5%8C%96%E7%9A%84%E8%AE%BE%E5%A4%87%E4%B8%8A%E5%90%AF%E7%94%A8%E8%BF%9C%E7%A8%8B%E6%A1%8C%E9%9D%A2/CleanShot%202024-12-04%20at%2009.47.55@2x_K6ghHdRIC6.avif)

如果此时你觉得他对你造成了不便，你可以在微软账号管理网页中重新添加密码

稍等后重启电脑就可以重新看到密码选项了，关闭仅允许使用 Windows Hello 登陆后就可以重新使用微软账号密码进行远程桌面了

![](https://blog-img.mitsea.com/images/blog/posts/2024/12/%E5%9C%A8%E4%BD%BF%E7%94%A8%E5%BE%AE%E8%BD%AF%E8%B4%A6%E5%8F%B7%E5%88%9D%E5%A7%8B%E5%8C%96%E7%9A%84%E8%AE%BE%E5%A4%87%E4%B8%8A%E5%90%AF%E7%94%A8%E8%BF%9C%E7%A8%8B%E6%A1%8C%E9%9D%A2/CleanShot%202024-12-04%20at%2010.15.00@2x_bpcKM3ktp0.avif)

另外，直接给默认账户添加密码是不行的。换句话说，你可以先创建本地账号密码然后再登陆微软账号，但是不能先使用微软账号登录然后直接给本地账户添加密码。

![](https://blog-img.mitsea.com/images/blog/posts/2024/12/%E5%9C%A8%E4%BD%BF%E7%94%A8%E5%BE%AE%E8%BD%AF%E8%B4%A6%E5%8F%B7%E5%88%9D%E5%A7%8B%E5%8C%96%E7%9A%84%E8%AE%BE%E5%A4%87%E4%B8%8A%E5%90%AF%E7%94%A8%E8%BF%9C%E7%A8%8B%E6%A1%8C%E9%9D%A2/CleanShot%202024-12-04%20at%2009.46.26@2x_uWPpT7sNbV.avif)

## 其他

### oobe 时创建本地账号

下次重装系统时，可以考虑先创建本地账号，然后进了系统后，再登陆微软账号

由于本地账号实际上是属于一个叫 WORKSPACE 的域，所以配置的时候走工作配置但是配置本地域就可以了

1. 在这里选择注册工作或学校账户

   ![](https://blog-img.mitsea.com/images/blog/posts/2024/12/%E5%9C%A8%E4%BD%BF%E7%94%A8%E5%BE%AE%E8%BD%AF%E8%B4%A6%E5%8F%B7%E5%88%9D%E5%A7%8B%E5%8C%96%E7%9A%84%E8%AE%BE%E5%A4%87%E4%B8%8A%E5%90%AF%E7%94%A8%E8%BF%9C%E7%A8%8B%E6%A1%8C%E9%9D%A2/CleanShot%202024-12-03%20at%2017.36.30@2x_cLRxzIhMco.avif)
2. 点击登陆选项

   ![](https://blog-img.mitsea.com/images/blog/posts/2024/12/%E5%9C%A8%E4%BD%BF%E7%94%A8%E5%BE%AE%E8%BD%AF%E8%B4%A6%E5%8F%B7%E5%88%9D%E5%A7%8B%E5%8C%96%E7%9A%84%E8%AE%BE%E5%A4%87%E4%B8%8A%E5%90%AF%E7%94%A8%E8%BF%9C%E7%A8%8B%E6%A1%8C%E9%9D%A2/CleanShot%202024-12-03%20at%2017.36.54@2x_12UBOQRuBy.avif)
3. 改为域加入

   ![](https://blog-img.mitsea.com/images/blog/posts/2024/12/%E5%9C%A8%E4%BD%BF%E7%94%A8%E5%BE%AE%E8%BD%AF%E8%B4%A6%E5%8F%B7%E5%88%9D%E5%A7%8B%E5%8C%96%E7%9A%84%E8%AE%BE%E5%A4%87%E4%B8%8A%E5%90%AF%E7%94%A8%E8%BF%9C%E7%A8%8B%E6%A1%8C%E9%9D%A2/CleanShot%202024-12-03%20at%2017.38.12@2x_OEVmcmNKqc.avif)
4. 这里就可以正常创建一个本地账号了

   ![](https://blog-img.mitsea.com/images/blog/posts/2024/12/%E5%9C%A8%E4%BD%BF%E7%94%A8%E5%BE%AE%E8%BD%AF%E8%B4%A6%E5%8F%B7%E5%88%9D%E5%A7%8B%E5%8C%96%E7%9A%84%E8%AE%BE%E5%A4%87%E4%B8%8A%E5%90%AF%E7%94%A8%E8%BF%9C%E7%A8%8B%E6%A1%8C%E9%9D%A2/CleanShot%202024-12-03%20at%2017.38.38@2x_8zvkGPAcsW.avif)


> Photo by [Pawel Czerwinski](https://unsplash.com/@pawel_czerwinski?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/a-close-up-of-a-wall-with-a-checkered-pattern-wSWJo7cTZNA?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)
      