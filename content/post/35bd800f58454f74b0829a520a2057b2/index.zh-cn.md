+++
author = "FlintyLemming"
title = "【归档】如何抓取 iOS app 的安装包"
slug = "35bd800f58454f74b0829a520a2057b2"
date = "2019-10-09"
description = ""
categories = ["Apple"]
tags = ["iOS"]
image = "https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2019/10/%E5%A6%82%E4%BD%95%E6%8A%93%E5%8F%96%20iOS%20app%20%E7%9A%84%E5%AE%89%E8%A3%85%E5%8C%85/title.avif"
+++

iOS 从 AppStore 下载安装 app 虽然整个过程内容看似不可见，但本质就是下载一个 ipa 文件并安装，和 Android 类似。
这里介绍一下如何获取这个 ipa 文件。

方法一：利用老版本 iTunes 获取，这个过程很简单，重要是免费。我不过多介绍，可以看一下这篇文章：[https://www.jianshu.com/p/f4328cf81e4f](https://www.jianshu.com/p/f4328cf81e4f)

方法二：利用抓包软件获取。这个方法的缺点就是抓包软件基本都是收费的（当然你可以用共享账号下载，不过还是鼓励有条件支持正版），并且在国区下载不到，但是好处是可以在手机平板上完成。
下面介绍步骤：

1. 从美区（或者其他区域，非国区）下载 Thor
2. 点击这个图标，开始抓包。提示 HTTPS 解析设置，选择以后再说；提示配置 VPN，验证密码或者指纹添加。
    
    ![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2019/10/%E5%A6%82%E4%BD%95%E6%8A%93%E5%8F%96%20iOS%20app%20%E7%9A%84%E5%AE%89%E8%A3%85%E5%8C%85/1.avif)
    
3. 开启后，按钮会变成红色，状态栏会有 VPN 标志
    
    ![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2019/10/%E5%A6%82%E4%BD%95%E6%8A%93%E5%8F%96%20iOS%20app%20%E7%9A%84%E5%AE%89%E8%A3%85%E5%8C%85/2.avif)
    
4. 打开 AppStore ，下载一个 app ，等到有下载进度条的时候即可取消。
5. 返回 Thor ，关闭开关，点击下方的“抓包记录”，进入最近的一条
    
    ![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2019/10/%E5%A6%82%E4%BD%95%E6%8A%93%E5%8F%96%20iOS%20app%20%E7%9A%84%E5%AE%89%E8%A3%85%E5%8C%85/3.avif)
    
6. 这里我们就能看到一条文件格式为 .ipa 的下载链接，点进去
7. 点击链接-导出原始链接-拷贝
8. 然后我们就可以拷贝到任意的下载器下载了，这里用的 Shu
    
    ![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2019/10/%E5%A6%82%E4%BD%95%E6%8A%93%E5%8F%96%20iOS%20app%20%E7%9A%84%E5%AE%89%E8%A3%85%E5%8C%85/4.avif)
    
9. 这样就可以获得目标 ipa 了

方法三：参考下面这篇文章使用iMazing获得
[https://www.flinty.moe/ios-multi-apps/](https://www.flinty.moe/ios-multi-apps/)