+++
author = "FlintyLemming"
title = "不退出 App Store 当前账号 安装其他账号 App"
slug = "49741bb2e38a469fa9bc92d15ad519dd"
date = "2021-08-31"
description = "iOS 其实一直都内置多账号系统"
categories = ["Apple"]
tags = ["AppStore"]
image = "https://img.mitsea.com/blog/posts/2021/08/%E4%B8%8D%E9%80%80%E5%87%BA%20App%20Store%20%E5%BD%93%E5%89%8D%E8%B4%A6%E5%8F%B7%20%E5%AE%89%E8%A3%85%E5%85%B6%E4%BB%96%E8%B4%A6%E5%8F%B7%20App/title.avif"
+++

Apple Music 有个很坑人的设定，当你切换 App Store 账号的时候，缓存的来自 Apple Music 曲库的歌曲就会全部消失。当你缓存很多歌曲后，想要切换账号去别的区下个 App 的时候就会非常痛苦。但其实不需要退出当前账号，就可以通过工具部署其他账号的 App。下面通过 iMazing（Windows 适用）和 Apple Configurator 2（macOS 适用）介绍下方法。

# 前提条件

## 当前设备的 App Store 登录过相应的 Apple ID

这里只要求登录过即可，比如你一开始用的国区账号，然后登录一次美区账号，再切回来就行。

因为 App Store 其实一直都内置多账号系统，登录过后即便退出也会保存账号信息。

## 已经获取过对应 App

如果你只是要下载一个之前下过的 App，那就没有问题。但如果是要下个新 App，就比较麻烦了。这边我提供几个方案

### 使用 Windows 上的老版本 iTunes（最靠谱）

1. 从这里下载仍然包含 App Store 商店版本的 iTunes
    
    [使用 iTunes 在业务环境中部署应用](https://support.apple.com/zh-cn/HT208079)
    
2. 安装并打开，点击左上角的 音乐 - 编辑菜单..
    
    ![](https://img.mitsea.com/blog/posts/2021/08/%E4%B8%8D%E9%80%80%E5%87%BA%20App%20Store%20%E5%BD%93%E5%89%8D%E8%B4%A6%E5%8F%B7%20%E5%AE%89%E8%A3%85%E5%85%B6%E4%BB%96%E8%B4%A6%E5%8F%B7%20App/1.avif)
    
3. 勾选“应用” - 完成。并切换到刚才选择的 应用 菜单。
    
    ![](https://img.mitsea.com/blog/posts/2021/08/%E4%B8%8D%E9%80%80%E5%87%BA%20App%20Store%20%E5%BD%93%E5%89%8D%E8%B4%A6%E5%8F%B7%20%E5%AE%89%E8%A3%85%E5%85%B6%E4%BB%96%E8%B4%A6%E5%8F%B7%20App/2.avif)
    
4. 点击这两个地方，都可以跳转到商店页面
    
    ![](https://img.mitsea.com/blog/posts/2021/08/%E4%B8%8D%E9%80%80%E5%87%BA%20App%20Store%20%E5%BD%93%E5%89%8D%E8%B4%A6%E5%8F%B7%20%E5%AE%89%E8%A3%85%E5%85%B6%E4%BB%96%E8%B4%A6%E5%8F%B7%20App/3.avif)
    
5. 点击 账户 - 登录，登录需要获取新应用的 Apple ID
    
    ![](https://img.mitsea.com/blog/posts/2021/08/%E4%B8%8D%E9%80%80%E5%87%BA%20App%20Store%20%E5%BD%93%E5%89%8D%E8%B4%A6%E5%8F%B7%20%E5%AE%89%E8%A3%85%E5%85%B6%E4%BB%96%E8%B4%A6%E5%8F%B7%20App/4.avif)
    
6. 与移动端类似，点击一个 App，就可以 “Get” 了
    
    ![](https://img.mitsea.com/blog/posts/2021/08/%E4%B8%8D%E9%80%80%E5%87%BA%20App%20Store%20%E5%BD%93%E5%89%8D%E8%B4%A6%E5%8F%B7%20%E5%AE%89%E8%A3%85%E5%85%B6%E4%BB%96%E8%B4%A6%E5%8F%B7%20App/5.avif)
    

### 使用 M1 Mac 的 App Store 获取 App

M1 Mac 的 App Store 可以检索 iPhone 和 iPad 程序，当然也可以获取新应用。不过有的 App 并不允许在 M1 Mac 上运行，则会搜索不到。

![](https://img.mitsea.com/blog/posts/2021/08/%E4%B8%8D%E9%80%80%E5%87%BA%20App%20Store%20%E5%BD%93%E5%89%8D%E8%B4%A6%E5%8F%B7%20%E5%AE%89%E8%A3%85%E5%85%B6%E4%BB%96%E8%B4%A6%E5%8F%B7%20App/6.avif)

# 具体步骤

## 使用 Apple Configurator 2（macOS 适用）

1. 连接设备到电脑，打开 Apple Configurator 2，双击已经连接的设备
    
    ![](https://img.mitsea.com/blog/posts/2021/08/%E4%B8%8D%E9%80%80%E5%87%BA%20App%20Store%20%E5%BD%93%E5%89%8D%E8%B4%A6%E5%8F%B7%20%E5%AE%89%E8%A3%85%E5%85%B6%E4%BB%96%E8%B4%A6%E5%8F%B7%20App/7.avif)
    
2. 菜单栏找到 账户，登录需要下载的 App 所在的账户
3. 添加 - App
    
    ![](https://img.mitsea.com/blog/posts/2021/08/%E4%B8%8D%E9%80%80%E5%87%BA%20App%20Store%20%E5%BD%93%E5%89%8D%E8%B4%A6%E5%8F%B7%20%E5%AE%89%E8%A3%85%E5%85%B6%E4%BB%96%E8%B4%A6%E5%8F%B7%20App/8.avif)
    
4. 找到需要安装的 App，点击 添加
    
    ![](https://img.mitsea.com/blog/posts/2021/08/%E4%B8%8D%E9%80%80%E5%87%BA%20App%20Store%20%E5%BD%93%E5%89%8D%E8%B4%A6%E5%8F%B7%20%E5%AE%89%E8%A3%85%E5%85%B6%E4%BB%96%E8%B4%A6%E5%8F%B7%20App/9.avif)
    
5. 稍等他下载并安装完成
    
    ![](https://img.mitsea.com/blog/posts/2021/08/%E4%B8%8D%E9%80%80%E5%87%BA%20App%20Store%20%E5%BD%93%E5%89%8D%E8%B4%A6%E5%8F%B7%20%E5%AE%89%E8%A3%85%E5%85%B6%E4%BB%96%E8%B4%A6%E5%8F%B7%20App/10.avif)
    

## 使用 iMazing 2（Windows、macOS 适用）

1. 连接设备后，点击 管理应用程序
    
    ![](https://img.mitsea.com/blog/posts/2021/08/%E4%B8%8D%E9%80%80%E5%87%BA%20App%20Store%20%E5%BD%93%E5%89%8D%E8%B4%A6%E5%8F%B7%20%E5%AE%89%E8%A3%85%E5%85%B6%E4%BB%96%E8%B4%A6%E5%8F%B7%20App/11.avif)
    
2. 左上角，切换到 资料库
    
    ![](https://img.mitsea.com/blog/posts/2021/08/%E4%B8%8D%E9%80%80%E5%87%BA%20App%20Store%20%E5%BD%93%E5%89%8D%E8%B4%A6%E5%8F%B7%20%E5%AE%89%E8%A3%85%E5%85%B6%E4%BB%96%E8%B4%A6%E5%8F%B7%20App/12.avif)
    
3. 默认的话，应该是你当前设备登录的 Apple ID。可以点击右上角的 退出，然后换成别的 Apple ID。
4. 找到需要安装的 App，选择后点击 安装 即可
    
    ![](https://img.mitsea.com/blog/posts/2021/08/%E4%B8%8D%E9%80%80%E5%87%BA%20App%20Store%20%E5%BD%93%E5%89%8D%E8%B4%A6%E5%8F%B7%20%E5%AE%89%E8%A3%85%E5%85%B6%E4%BB%96%E8%B4%A6%E5%8F%B7%20App/13.avif)
    

# Q&A

### Q：为什么下载的 App 点开后需要输入账号密码

A：当前设备的 App Store 之前没有登录过对应账号，请按照教程严格操作

### Q：为什么更新时需要输入密码

A：当你的 App Store 从 A 账号切换到 B 账号后，第一次更新 A 账号的应用时，需要输入一遍 A 账号的密码。这不会退出你当前 App Store 的账号。