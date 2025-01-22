+++
author = "FlintyLemming"
title = "关于 Microsoft 365 世纪互联的权限说明"
slug = "cec31fd42c554e94a43e7979ee6a6b2c"
date = "2020-03-07"
description = ""
categories = ["Microsoft"]
tags = ["Microsoft 365", "Azure"]
image = "https://blog-img.mitsea.com/images/blog/posts/2020/03/%E5%85%B3%E4%BA%8E%20Microsoft%20365%20%E4%B8%96%E7%BA%AA%E4%BA%92%E8%81%94%E7%9A%84%E6%9D%83%E9%99%90%E8%AF%B4%E6%98%8E/title.avif"
+++

众所周知，Microsoft 365 世纪互联版是国内代理服务，在提供高速服务的同时受到中国法律的监督。所以在权限等可访问性上与国际版不同，下面就核心的几个功能做一个对比。

简单说一下两者的版本，世纪互联版买的是 Microsoft 365 商业版，国际版是 Developer E3。

## OneDrive

国际版的 OneDrive 自然是可以直接与他人共享，只需要将共享链接发送给别人，别人即可免登陆直接下载，就是速度不怎么快。

世纪互联版的话，这项功能默认是关闭的，可以通过下面的设置步骤打开。

1. 进入功能列表 - 管理

    ![](https://blog-img.mitsea.com/images/blog/posts/2020/03/%E5%85%B3%E4%BA%8E%20Microsoft%20365%20%E4%B8%96%E7%BA%AA%E4%BA%92%E8%81%94%E7%9A%84%E6%9D%83%E9%99%90%E8%AF%B4%E6%98%8E/1.avif)

2. 所有管理中心 - SharePoint

    ![](https://blog-img.mitsea.com/images/blog/posts/2020/03/%E5%85%B3%E4%BA%8E%20Microsoft%20365%20%E4%B8%96%E7%BA%AA%E4%BA%92%E8%81%94%E7%9A%84%E6%9D%83%E9%99%90%E8%AF%B4%E6%98%8E/2.avif)

3. 策略 - 共享 里，像这样修改共享设置

    ![](https://blog-img.mitsea.com/images/blog/posts/2020/03/%E5%85%B3%E4%BA%8E%20Microsoft%20365%20%E4%B8%96%E7%BA%AA%E4%BA%92%E8%81%94%E7%9A%84%E6%9D%83%E9%99%90%E8%AF%B4%E6%98%8E/3.avif)

4. 然后就可以正常分享文件了

    ![](https://blog-img.mitsea.com/images/blog/posts/2020/03/%E5%85%B3%E4%BA%8E%20Microsoft%20365%20%E4%B8%96%E7%BA%AA%E4%BA%92%E8%81%94%E7%9A%84%E6%9D%83%E9%99%90%E8%AF%B4%E6%98%8E/4.avif)

## SharePoint

SharePoint 不能够分享整个站点给任意用户。但从原理上说，你可以通过邀请的方式将一个陌生人加入你的站点访问列表，这样他也就可以查看整个站点了。下面说下设置步骤（但这项功能在世纪互联版暂不可用，后面会解释）

1. 进入要共享的站点，然后点击设置齿轮 - 网站设置

    ![](https://blog-img.mitsea.com/images/blog/posts/2020/03/%E5%85%B3%E4%BA%8E%20Microsoft%20365%20%E4%B8%96%E7%BA%AA%E4%BA%92%E8%81%94%E7%9A%84%E6%9D%83%E9%99%90%E8%AF%B4%E6%98%8E/5.avif)

2. 点击 网站权限

    ![](https://blog-img.mitsea.com/images/blog/posts/2020/03/%E5%85%B3%E4%BA%8E%20Microsoft%20365%20%E4%B8%96%E7%BA%AA%E4%BA%92%E8%81%94%E7%9A%84%E6%9D%83%E9%99%90%E8%AF%B4%E6%98%8E/6.avif)

3. 左上角 权限 - 授予权限

    ![](https://blog-img.mitsea.com/images/blog/posts/2020/03/%E5%85%B3%E4%BA%8E%20Microsoft%20365%20%E4%B8%96%E7%BA%AA%E4%BA%92%E8%81%94%E7%9A%84%E6%9D%83%E9%99%90%E8%AF%B4%E6%98%8E/7.avif)

4. 在这里输入对方的 Microsoft 账号（国际版也可以），然后对方就会收到邀请链接

    ![](https://blog-img.mitsea.com/images/blog/posts/2020/03/%E5%85%B3%E4%BA%8E%20Microsoft%20365%20%E4%B8%96%E7%BA%AA%E4%BA%92%E8%81%94%E7%9A%84%E6%9D%83%E9%99%90%E8%AF%B4%E6%98%8E/8.avif)

但是这项功能世纪互联版暂时是不可用的，这里引用客服的回答

> 如我们在电话中所沟通的，很抱歉，世纪互联版SharePoint online服务不支持SharePoint站点的匿名共享，也就是说将网站共享给组织以外的用户，让外部用户免登录去访问这个网站，这个功能是不支持的。
世纪互联版SharePoint online服务支持的网站共享方式邀请来宾用户的方式，就是邀请拥有Microsoft账户的用户访问SharePoint网站，经过Microsoft账户验证后，是可以访问共享的网站的，但是很抱歉的是，当前邀请来宾用户的功能有些问题，暂不可用，后台服务团队一直在修复这个问题，还没有完全修复，所以邀请来宾用户的方式共享网站暂时也用不了。
但是您可以通过匿名的方式共享SharePoint或者OneDrive的文档给外部用户，这样外部用户是可以直接打开共享的文件的。

Photo by [Matthew Manuel](https://unsplash.com/@sawtooth_utopia?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/microsoft?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)