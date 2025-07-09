+++
author = "FlintyLemming"
title = "拷打付费软件 —— iShell Pro"
slug = "8QEVboMedjqvL6AS9MCeaL"
date = "2025-01-25"
description = "第三款软件"
categories = ["MineService"]
tags = ["SSH"]
image = "https://img.mitsea.com/blog/posts/2025/01/%E6%8B%B7%E6%89%93%E4%BB%98%E8%B4%B9%E8%BD%AF%E4%BB%B6%20%E2%80%94%E2%80%94%20iShell%20Pro/milad-fakurian-cTlimlJPNE4-unsplash.avif"
+++

本来不想批判这个软件的，因为可吐槽的点实在是太多，我都懒得写。但是我看到它这个状态就开始做推广，还有很多人买，我真的是想能劝一个是一个。

![](https://img.mitsea.com/blog/posts/2025/01/%E6%8B%B7%E6%89%93%E4%BB%98%E8%B4%B9%E8%BD%AF%E4%BB%B6%20%E2%80%94%E2%80%94%20iShell%20Pro/image-20250125193027-njz9q7c.avif)

简单来说，这个软件就是由于开发既不懂 Flutter 开发，对 Linux 操作也不太懂，两个问题共同导致的一个灾难。

这个软件终身售价目前是 199，既然是收钱了，而且是放在官网上的正式版。所以官方对于存在问题的一切理由比如什么换了新架构重新开发、不熟悉什么的我都不接受。咱们这个系列的一个目的就是要狠狠批判那些画大饼的收费软件。下面直接开始

### 现有功能的问题

1. AI 工具没有新建会话，就一个弹出框，除了文本框没有任何其他按钮

    ![](https://img.mitsea.com/blog/posts/2025/01/%E6%8B%B7%E6%89%93%E4%BB%98%E8%B4%B9%E8%BD%AF%E4%BB%B6%20%E2%80%94%E2%80%94%20iShell%20Pro/image-20250125183410-xoyu1xu.avif)

2. 字体设置没有预设常用字体，比如 Ubuntu Mono

    ![](https://img.mitsea.com/blog/posts/2025/01/%E6%8B%B7%E6%89%93%E4%BB%98%E8%B4%B9%E8%BD%AF%E4%BB%B6%20%E2%80%94%E2%80%94%20iShell%20Pro/image-20250125183514-3bjnc1i.avif)

    而且就算你在系统里装了，选择字体后也是无效的。比如我终端字体选择了 Ubuntu Mono，并重启软件，但实际上终端里面的字体根本就不是这个字体。Ubuntu Mono 的 g 根本就不是这样。

    ![](https://img.mitsea.com/blog/posts/2025/01/%E6%8B%B7%E6%89%93%E4%BB%98%E8%B4%B9%E8%BD%AF%E4%BB%B6%20%E2%80%94%E2%80%94%20iShell%20Pro/image-20250125185444-lu3q0ue.avif)

3. 字体预览无效

    ![](https://img.mitsea.com/blog/posts/2025/01/%E6%8B%B7%E6%89%93%E4%BB%98%E8%B4%B9%E8%BD%AF%E4%BB%B6%20%E2%80%94%E2%80%94%20iShell%20Pro/image-20250125183842-r7408ha.avif)

4. 系统字体设置意义不明，设置了 Ubuntu 字体，下面的路径英文也没有变化

    ![](https://img.mitsea.com/blog/posts/2025/01/%E6%8B%B7%E6%89%93%E4%BB%98%E8%B4%B9%E8%BD%AF%E4%BB%B6%20%E2%80%94%E2%80%94%20iShell%20Pro/image-20250125184011-ruzwjcu.avif)

    字体问题还有很多，包括间距行距不能调，看着怪怪的，懒得一个一个列举了

5. 自动同步不工作

    他的设置里可以修改云同步的时机，close 和 change（这里的文本也没改，还就一个单词）。设置为 change 后您猜怎么着？还真就是只在发生修改时才同步，删除一个保存的 SSH 连接的时候是不同步的，绷不住了……

    ![](https://img.mitsea.com/blog/posts/2025/01/%E6%8B%B7%E6%89%93%E4%BB%98%E8%B4%B9%E8%BD%AF%E4%BB%B6%20%E2%80%94%E2%80%94%20iShell%20Pro/image-20250125184649-szrz4ss.avif)

6. 服务器状态信息显示内容密度极其低下，信息也少，纯花瓶，对比一下右边的 Xterminal

    ![](https://img.mitsea.com/blog/posts/2025/01/%E6%8B%B7%E6%89%93%E4%BB%98%E8%B4%B9%E8%BD%AF%E4%BB%B6%20%E2%80%94%E2%80%94%20iShell%20Pro/image-20250125185629-r28dhnx.avif)

7. 终端渲染问题一大堆

    比如这个连续深色色块渲染问题

    ![](https://img.mitsea.com/blog/posts/2025/01/%E6%8B%B7%E6%89%93%E4%BB%98%E8%B4%B9%E8%BD%AF%E4%BB%B6%20%E2%80%94%E2%80%94%20iShell%20Pro/image-20250125185738-3vw3x7z.avif)

    窗口发生变化的时候处理的也有问题，比如窗口化是这样的状态

    ![](https://img.mitsea.com/blog/posts/2025/01/%E6%8B%B7%E6%89%93%E4%BB%98%E8%B4%B9%E8%BD%AF%E4%BB%B6%20%E2%80%94%E2%80%94%20iShell%20Pro/image-20250125185903-88tx3qm.avif)

    全屏后又重新渲染了一遍当前行，导致多了一行出来

    ![](https://img.mitsea.com/blog/posts/2025/01/%E6%8B%B7%E6%89%93%E4%BB%98%E8%B4%B9%E8%BD%AF%E4%BB%B6%20%E2%80%94%E2%80%94%20iShell%20Pro/image-20250125185940-5qjxc08.avif)

    渲染问题居多无比，懒得一个一个列了

8. 终端交互问题

    当你的焦点在终端的时候按下 Ctrl+A，您猜怎么着？他真的全选了当前终端里的文本。不是哥们？那你告诉我 screen 怎么退出？

    ![](https://img.mitsea.com/blog/posts/2025/01/%E6%8B%B7%E6%89%93%E4%BB%98%E8%B4%B9%E8%BD%AF%E4%BB%B6%20%E2%80%94%E2%80%94%20iShell%20Pro/image-20250125190125-p5ze00l.avif)

9. 安装方法混乱，开发完全不懂 Windows 的权限管理

    上来就要个 UAC

    ![](https://img.mitsea.com/blog/posts/2025/01/%E6%8B%B7%E6%89%93%E4%BB%98%E8%B4%B9%E8%BD%AF%E4%BB%B6%20%E2%80%94%E2%80%94%20iShell%20Pro/image-20250125190615-4xodq1o.avif)

    然后到这里彻底傻眼了。首先路径默认是分区的根目录，不是哥们？那我要是有个 D 盘，你默认路径不能是 D:\IShellPro 吧？

    还有谁跟你说安装到 C 盘运行的时候软件运行就一定需要管理员权限？就连软件安装也不需要管理员啊，用户安装到自己用户目录下的 AppData 文件夹，要个毛线的 UAC 啊？

    ![](https://img.mitsea.com/blog/posts/2025/01/%E6%8B%B7%E6%89%93%E4%BB%98%E8%B4%B9%E8%BD%AF%E4%BB%B6%20%E2%80%94%E2%80%94%20iShell%20Pro/image-20250125190659-fsuwbsn.avif)

    对比一下别的软件的安装程序，先问你安装到哪里，只要不是全局安装（一般就是 Program Files 里）根本就不需要管理员权限啊

    ![](https://img.mitsea.com/blog/posts/2025/01/%E6%8B%B7%E6%89%93%E4%BB%98%E8%B4%B9%E8%BD%AF%E4%BB%B6%20%E2%80%94%E2%80%94%20iShell%20Pro/image-20250125190944-2ya0tt0.avif)

    以及这玩意的程序数据还是保存在用户的文档文件夹里，真的是没绷住

10. 开发者没有任何认证

     正如上面的 UAC 弹窗显示，发布者都懒得写一个。然后 macOS 那边呢，开发者付费证书自然是没有的，打开还得让用户自己在隐私设置里手动打开，哥们你要是开源软件就算了。
11. 相信看到上面这些问题你打开早就绷不住了，别急，最绷不住的是

     我当时闲着蛋疼买这个软件是冲着它说是会给会员提供同步的服务端程序可以自己部署。当你看到软件的这些问题后，你觉得他们多久会兑现这个功能？

### 功能缺失问题

这部分我真是懒得说，因为完全说不完啊

1. 列表没有排序功能

    没错，这上面一排点击都是没反应的

    ![](https://img.mitsea.com/blog/posts/2025/01/%E6%8B%B7%E6%89%93%E4%BB%98%E8%B4%B9%E8%BD%AF%E4%BB%B6%20%E2%80%94%E2%80%94%20iShell%20Pro/image-20250125191717-yptopd6.avif)

2. 性能监测缺了一堆东西，网络、GPU，通通没有
3. 没有快捷复制密码的按钮，对于非 root 用户经常需要输入密码非常不方便

    ![](https://img.mitsea.com/blog/posts/2025/01/%E6%8B%B7%E6%89%93%E4%BB%98%E8%B4%B9%E8%BD%AF%E4%BB%B6%20%E2%80%94%E2%80%94%20iShell%20Pro/image-20250125192708-k1ctajc.avif)

4. 不能设置跳板机

唉功能跟同价位的 Xterminal 差的太多太多了，懒得吐槽了

> ‍Photo by [Milad Fakurian](https://unsplash.com/@fakurian?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/a-close-up-of-a-cell-phone-with-a-blurry-background-cTlimlJPNE4?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)
      