+++
author = "FlintyLemming"
title = "Apple 开发者账号签名 ipa 并直接安装到指定设备中"
slug = "b7c9c8a1d2554fcd9acbf5ae7be3fd83"
date = "2022-04-19"
description = ""
categories = ["Apple", "Coding"]
tags = ["ipa", "Apple"]
image = "https://img.mitsea.com/blog/posts/2022/04/Apple%20%E5%BC%80%E5%8F%91%E8%80%85%E8%B4%A6%E5%8F%B7%E7%AD%BE%E5%90%8D%20ipa%20%E5%B9%B6%E7%9B%B4%E6%8E%A5%E5%AE%89%E8%A3%85%E5%88%B0%E6%8C%87%E5%AE%9A%E8%AE%BE%E5%A4%87%E4%B8%AD/susan-wilkinson-2o5W4PgqjRQ-unsplash.jpg?x-oss-process=style/ImageCompress"
+++

开发中有的时候需要把 ipa 发给客户先看看。但是弄 TestFlight 又有点麻烦，因为要避免审核的话，还得拉一个内部测试组。所以用 Ad Hoc 直接分发 ipa 安装是比较方便的做法。

# 准备 App 和签名需要的文件

## 获取证书（p12 文件）

1. 打开 钥匙串访问 App，菜单栏找到 钥匙串访问 - 证书助理 - 从证书颁发机构请求证书...

    ![](https://img.mitsea.com/blog/posts/2022/04/Apple%20%E5%BC%80%E5%8F%91%E8%80%85%E8%B4%A6%E5%8F%B7%E7%AD%BE%E5%90%8D%20ipa%20%E5%B9%B6%E7%9B%B4%E6%8E%A5%E5%AE%89%E8%A3%85%E5%88%B0%E6%8C%87%E5%AE%9A%E8%AE%BE%E5%A4%87%E4%B8%AD/Untitled.png?x-oss-process=style/ImageCompress)

2. 填一下邮件地址和常用名称，选择“存储到磁盘”，点击继续

    ![](https://img.mitsea.com/blog/posts/2022/04/Apple%20%E5%BC%80%E5%8F%91%E8%80%85%E8%B4%A6%E5%8F%B7%E7%AD%BE%E5%90%8D%20ipa%20%E5%B9%B6%E7%9B%B4%E6%8E%A5%E5%AE%89%E8%A3%85%E5%88%B0%E6%8C%87%E5%AE%9A%E8%AE%BE%E5%A4%87%E4%B8%AD%20b7c9c8a1d2554fcd9acbf5ae7be3fd83/Untitled%201.png?x-oss-process=style/ImageCompress)

3. 获得请求文件

    ![](https://img.mitsea.com/blog/posts/2022/04/Apple%20%E5%BC%80%E5%8F%91%E8%80%85%E8%B4%A6%E5%8F%B7%E7%AD%BE%E5%90%8D%20ipa%20%E5%B9%B6%E7%9B%B4%E6%8E%A5%E5%AE%89%E8%A3%85%E5%88%B0%E6%8C%87%E5%AE%9A%E8%AE%BE%E5%A4%87%E4%B8%AD%20b7c9c8a1d2554fcd9acbf5ae7be3fd83/Untitled%202.png?x-oss-process=style/ImageCompress)

4. 进入开发者网页后台，在 Certificates, Identifiers & Profiles 里点击 Certificates 右边的加号

    ![](https://img.mitsea.com/blog/posts/2022/04/Apple%20%E5%BC%80%E5%8F%91%E8%80%85%E8%B4%A6%E5%8F%B7%E7%AD%BE%E5%90%8D%20ipa%20%E5%B9%B6%E7%9B%B4%E6%8E%A5%E5%AE%89%E8%A3%85%E5%88%B0%E6%8C%87%E5%AE%9A%E8%AE%BE%E5%A4%87%E4%B8%AD%20b7c9c8a1d2554fcd9acbf5ae7be3fd83/Untitled%203.png?x-oss-process=style/ImageCompress)

5. 目的是安装到一些指定设备上，属于 Ad Hoc 的类型，所以选择 iOS Distribution

    ![](https://img.mitsea.com/blog/posts/2022/04/Apple%20%E5%BC%80%E5%8F%91%E8%80%85%E8%B4%A6%E5%8F%B7%E7%AD%BE%E5%90%8D%20ipa%20%E5%B9%B6%E7%9B%B4%E6%8E%A5%E5%AE%89%E8%A3%85%E5%88%B0%E6%8C%87%E5%AE%9A%E8%AE%BE%E5%A4%87%E4%B8%AD%20b7c9c8a1d2554fcd9acbf5ae7be3fd83/Untitled%204.png?x-oss-process=style/ImageCompress)

6. 上传刚才生成的请求文件

    ![](https://img.mitsea.com/blog/posts/2022/04/Apple%20%E5%BC%80%E5%8F%91%E8%80%85%E8%B4%A6%E5%8F%B7%E7%AD%BE%E5%90%8D%20ipa%20%E5%B9%B6%E7%9B%B4%E6%8E%A5%E5%AE%89%E8%A3%85%E5%88%B0%E6%8C%87%E5%AE%9A%E8%AE%BE%E5%A4%87%E4%B8%AD%20b7c9c8a1d2554fcd9acbf5ae7be3fd83/Untitled%205.png?x-oss-process=style/ImageCompress)

7. 下载证书

    ![](https://img.mitsea.com/blog/posts/2022/04/Apple%20%E5%BC%80%E5%8F%91%E8%80%85%E8%B4%A6%E5%8F%B7%E7%AD%BE%E5%90%8D%20ipa%20%E5%B9%B6%E7%9B%B4%E6%8E%A5%E5%AE%89%E8%A3%85%E5%88%B0%E6%8C%87%E5%AE%9A%E8%AE%BE%E5%A4%87%E4%B8%AD%20b7c9c8a1d2554fcd9acbf5ae7be3fd83/Untitled%206.png?x-oss-process=style/ImageCompress)

8. 再次回到钥匙串访问 App 里，选择证书，然后把刚才生成的证书拖进来。右键拖进来的证书，选择导出

    ![](https://img.mitsea.com/blog/posts/2022/04/Apple%20%E5%BC%80%E5%8F%91%E8%80%85%E8%B4%A6%E5%8F%B7%E7%AD%BE%E5%90%8D%20ipa%20%E5%B9%B6%E7%9B%B4%E6%8E%A5%E5%AE%89%E8%A3%85%E5%88%B0%E6%8C%87%E5%AE%9A%E8%AE%BE%E5%A4%87%E4%B8%AD%20b7c9c8a1d2554fcd9acbf5ae7be3fd83/Untitled%207.png?x-oss-process=style/ImageCompress)

9. 选择一个保存位置后，需要你设置一个密码。这个密码要记住后面要用。

    ![](https://img.mitsea.com/blog/posts/2022/04/Apple%20%E5%BC%80%E5%8F%91%E8%80%85%E8%B4%A6%E5%8F%B7%E7%AD%BE%E5%90%8D%20ipa%20%E5%B9%B6%E7%9B%B4%E6%8E%A5%E5%AE%89%E8%A3%85%E5%88%B0%E6%8C%87%E5%AE%9A%E8%AE%BE%E5%A4%87%E4%B8%AD%20b7c9c8a1d2554fcd9acbf5ae7be3fd83/Untitled%208.png?x-oss-process=style/ImageCompress)

10. 然后就获得了 p12 证书

    ![](https://img.mitsea.com/blog/posts/2022/04/Apple%20%E5%BC%80%E5%8F%91%E8%80%85%E8%B4%A6%E5%8F%B7%E7%AD%BE%E5%90%8D%20ipa%20%E5%B9%B6%E7%9B%B4%E6%8E%A5%E5%AE%89%E8%A3%85%E5%88%B0%E6%8C%87%E5%AE%9A%E8%AE%BE%E5%A4%87%E4%B8%AD%20b7c9c8a1d2554fcd9acbf5ae7be3fd83/Untitled%209.png?x-oss-process=style/ImageCompress)

## 获取描述文件

### 添加 Identifier

1. 回到 Certificates, Identifiers & Profiles 里，给 App 添加一个 Identifier

    ![](https://img.mitsea.com/blog/posts/2022/04/Apple%20%E5%BC%80%E5%8F%91%E8%80%85%E8%B4%A6%E5%8F%B7%E7%AD%BE%E5%90%8D%20ipa%20%E5%B9%B6%E7%9B%B4%E6%8E%A5%E5%AE%89%E8%A3%85%E5%88%B0%E6%8C%87%E5%AE%9A%E8%AE%BE%E5%A4%87%E4%B8%AD%20b7c9c8a1d2554fcd9acbf5ae7be3fd83/Untitled%2010.png?x-oss-process=style/ImageCompress)

2. 选择 App IDs

    ![](https://img.mitsea.com/blog/posts/2022/04/Apple%20%E5%BC%80%E5%8F%91%E8%80%85%E8%B4%A6%E5%8F%B7%E7%AD%BE%E5%90%8D%20ipa%20%E5%B9%B6%E7%9B%B4%E6%8E%A5%E5%AE%89%E8%A3%85%E5%88%B0%E6%8C%87%E5%AE%9A%E8%AE%BE%E5%A4%87%E4%B8%AD%20b7c9c8a1d2554fcd9acbf5ae7be3fd83/Untitled%2011.png?x-oss-process=style/ImageCompress)

3. 创建一个包名，下面的兼容性根据实际需要勾选

    ![](https://img.mitsea.com/blog/posts/2022/04/Apple%20%E5%BC%80%E5%8F%91%E8%80%85%E8%B4%A6%E5%8F%B7%E7%AD%BE%E5%90%8D%20ipa%20%E5%B9%B6%E7%9B%B4%E6%8E%A5%E5%AE%89%E8%A3%85%E5%88%B0%E6%8C%87%E5%AE%9A%E8%AE%BE%E5%A4%87%E4%B8%AD%20b7c9c8a1d2554fcd9acbf5ae7be3fd83/Untitled%2012.png?x-oss-process=style/ImageCompress)

### 添加设备

1. 点击 Devices 右侧的加号

    ![](https://img.mitsea.com/blog/posts/2022/04/Apple%20%E5%BC%80%E5%8F%91%E8%80%85%E8%B4%A6%E5%8F%B7%E7%AD%BE%E5%90%8D%20ipa%20%E5%B9%B6%E7%9B%B4%E6%8E%A5%E5%AE%89%E8%A3%85%E5%88%B0%E6%8C%87%E5%AE%9A%E8%AE%BE%E5%A4%87%E4%B8%AD%20b7c9c8a1d2554fcd9acbf5ae7be3fd83/Untitled%2013.png?x-oss-process=style/ImageCompress)

2. 获取填写设备名称和 UDID

    ![](https://img.mitsea.com/blog/posts/2022/04/Apple%20%E5%BC%80%E5%8F%91%E8%80%85%E8%B4%A6%E5%8F%B7%E7%AD%BE%E5%90%8D%20ipa%20%E5%B9%B6%E7%9B%B4%E6%8E%A5%E5%AE%89%E8%A3%85%E5%88%B0%E6%8C%87%E5%AE%9A%E8%AE%BE%E5%A4%87%E4%B8%AD%20b7c9c8a1d2554fcd9acbf5ae7be3fd83/Untitled%2014.png?x-oss-process=style/ImageCompress)

### 添加 ****Profile****

1. 点击 Profiles 右边的加号

    ![](https://img.mitsea.com/blog/posts/2022/04/Apple%20%E5%BC%80%E5%8F%91%E8%80%85%E8%B4%A6%E5%8F%B7%E7%AD%BE%E5%90%8D%20ipa%20%E5%B9%B6%E7%9B%B4%E6%8E%A5%E5%AE%89%E8%A3%85%E5%88%B0%E6%8C%87%E5%AE%9A%E8%AE%BE%E5%A4%87%E4%B8%AD%20b7c9c8a1d2554fcd9acbf5ae7be3fd83/Untitled%2015.png?x-oss-process=style/ImageCompress)

2. 因为是分发给指定设备，所以选择 Ad Hoc

    ![](https://img.mitsea.com/blog/posts/2022/04/Apple%20%E5%BC%80%E5%8F%91%E8%80%85%E8%B4%A6%E5%8F%B7%E7%AD%BE%E5%90%8D%20ipa%20%E5%B9%B6%E7%9B%B4%E6%8E%A5%E5%AE%89%E8%A3%85%E5%88%B0%E6%8C%87%E5%AE%9A%E8%AE%BE%E5%A4%87%E4%B8%AD%20b7c9c8a1d2554fcd9acbf5ae7be3fd83/Untitled%2016.png?x-oss-process=style/ImageCompress)

3. 选择刚才在 Identifiers 添加的包名

    ![](https://img.mitsea.com/blog/posts/2022/04/Apple%20%E5%BC%80%E5%8F%91%E8%80%85%E8%B4%A6%E5%8F%B7%E7%AD%BE%E5%90%8D%20ipa%20%E5%B9%B6%E7%9B%B4%E6%8E%A5%E5%AE%89%E8%A3%85%E5%88%B0%E6%8C%87%E5%AE%9A%E8%AE%BE%E5%A4%87%E4%B8%AD%20b7c9c8a1d2554fcd9acbf5ae7be3fd83/Untitled%2017.png?x-oss-process=style/ImageCompress)

4. 选择证书

    ![](https://img.mitsea.com/blog/posts/2022/04/Apple%20%E5%BC%80%E5%8F%91%E8%80%85%E8%B4%A6%E5%8F%B7%E7%AD%BE%E5%90%8D%20ipa%20%E5%B9%B6%E7%9B%B4%E6%8E%A5%E5%AE%89%E8%A3%85%E5%88%B0%E6%8C%87%E5%AE%9A%E8%AE%BE%E5%A4%87%E4%B8%AD%20b7c9c8a1d2554fcd9acbf5ae7be3fd83/Untitled%2018.png?x-oss-process=style/ImageCompress)

5. 选择指定的设备

    ![](https://img.mitsea.com/blog/posts/2022/04/Apple%20%E5%BC%80%E5%8F%91%E8%80%85%E8%B4%A6%E5%8F%B7%E7%AD%BE%E5%90%8D%20ipa%20%E5%B9%B6%E7%9B%B4%E6%8E%A5%E5%AE%89%E8%A3%85%E5%88%B0%E6%8C%87%E5%AE%9A%E8%AE%BE%E5%A4%87%E4%B8%AD%20b7c9c8a1d2554fcd9acbf5ae7be3fd83/Untitled%2019.png?x-oss-process=style/ImageCompress)

6. 起个名字就可以生成描述文件了

    ![](https://img.mitsea.com/blog/posts/2022/04/Apple%20%E5%BC%80%E5%8F%91%E8%80%85%E8%B4%A6%E5%8F%B7%E7%AD%BE%E5%90%8D%20ipa%20%E5%B9%B6%E7%9B%B4%E6%8E%A5%E5%AE%89%E8%A3%85%E5%88%B0%E6%8C%87%E5%AE%9A%E8%AE%BE%E5%A4%87%E4%B8%AD%20b7c9c8a1d2554fcd9acbf5ae7be3fd83/Untitled%2020.png?x-oss-process=style/ImageCompress)

# 签名 App

通过上面的步骤，就准备好了签名必要的 p12 文件和描述文件

![](https://img.mitsea.com/blog/posts/2022/04/Apple%20%E5%BC%80%E5%8F%91%E8%80%85%E8%B4%A6%E5%8F%B7%E7%AD%BE%E5%90%8D%20ipa%20%E5%B9%B6%E7%9B%B4%E6%8E%A5%E5%AE%89%E8%A3%85%E5%88%B0%E6%8C%87%E5%AE%9A%E8%AE%BE%E5%A4%87%E4%B8%AD%20b7c9c8a1d2554fcd9acbf5ae7be3fd83/Untitled%2021.png?x-oss-process=style/ImageCompress)

签名的工具很多，macOS 可以用 iOS App Signer，Windows 可以用爱思助手

## iOS App Signer

![](https://img.mitsea.com/blog/posts/2022/04/Apple%20%E5%BC%80%E5%8F%91%E8%80%85%E8%B4%A6%E5%8F%B7%E7%AD%BE%E5%90%8D%20ipa%20%E5%B9%B6%E7%9B%B4%E6%8E%A5%E5%AE%89%E8%A3%85%E5%88%B0%E6%8C%87%E5%AE%9A%E8%AE%BE%E5%A4%87%E4%B8%AD%20b7c9c8a1d2554fcd9acbf5ae7be3fd83/Untitled%2022.png?x-oss-process=style/ImageCompress)

软件打开选择 ipa 后，Signing Certificate 选择之前导入到钥匙串的证书，Provisioning Profile 选择刚才下载的描述文件即可。

## 爱思助手

1. 打开 IPA 签名，添加 ipa 文件后，点击右下方的导入证书

    ![](https://img.mitsea.com/blog/posts/2022/04/Apple%20%E5%BC%80%E5%8F%91%E8%80%85%E8%B4%A6%E5%8F%B7%E7%AD%BE%E5%90%8D%20ipa%20%E5%B9%B6%E7%9B%B4%E6%8E%A5%E5%AE%89%E8%A3%85%E5%88%B0%E6%8C%87%E5%AE%9A%E8%AE%BE%E5%A4%87%E4%B8%AD%20b7c9c8a1d2554fcd9acbf5ae7be3fd83/Untitled%2023.png?x-oss-process=style/ImageCompress)

2. 填入准备好的 p12 证书、生成证书时设定的密码、描述文件

    ![](https://img.mitsea.com/blog/posts/2022/04/Apple%20%E5%BC%80%E5%8F%91%E8%80%85%E8%B4%A6%E5%8F%B7%E7%AD%BE%E5%90%8D%20ipa%20%E5%B9%B6%E7%9B%B4%E6%8E%A5%E5%AE%89%E8%A3%85%E5%88%B0%E6%8C%87%E5%AE%9A%E8%AE%BE%E5%A4%87%E4%B8%AD%20b7c9c8a1d2554fcd9acbf5ae7be3fd83/Untitled%2024.png?x-oss-process=style/ImageCompress)

3. 最后点击开始签名即可

    ![](https://img.mitsea.com/blog/posts/2022/04/Apple%20%E5%BC%80%E5%8F%91%E8%80%85%E8%B4%A6%E5%8F%B7%E7%AD%BE%E5%90%8D%20ipa%20%E5%B9%B6%E7%9B%B4%E6%8E%A5%E5%AE%89%E8%A3%85%E5%88%B0%E6%8C%87%E5%AE%9A%E8%AE%BE%E5%A4%87%E4%B8%AD%20b7c9c8a1d2554fcd9acbf5ae7be3fd83/Untitled%2025.png?x-oss-process=style/ImageCompress)

# 安装 App

安装方法有很多，我用的最多的方法是隔空投送。直接 AirDrop ipa 文件给注册过的设备，就会直接询问你是否安装，点击安装即可

![](https://img.mitsea.com/blog/posts/2022/04/Apple%20%E5%BC%80%E5%8F%91%E8%80%85%E8%B4%A6%E5%8F%B7%E7%AD%BE%E5%90%8D%20ipa%20%E5%B9%B6%E7%9B%B4%E6%8E%A5%E5%AE%89%E8%A3%85%E5%88%B0%E6%8C%87%E5%AE%9A%E8%AE%BE%E5%A4%87%E4%B8%AD%20b7c9c8a1d2554fcd9acbf5ae7be3fd83/Untitled%2026.png?x-oss-process=style/ImageCompress)

![](https://img.mitsea.com/blog/posts/2022/04/Apple%20%E5%BC%80%E5%8F%91%E8%80%85%E8%B4%A6%E5%8F%B7%E7%AD%BE%E5%90%8D%20ipa%20%E5%B9%B6%E7%9B%B4%E6%8E%A5%E5%AE%89%E8%A3%85%E5%88%B0%E6%8C%87%E5%AE%9A%E8%AE%BE%E5%A4%87%E4%B8%AD%20b7c9c8a1d2554fcd9acbf5ae7be3fd83/IMG_0112.png?x-oss-process=style/ImageCompress)

Windows 的话，用爱思助手也可以直接安装

> Photo by [Susan Wilkinson](https://unsplash.com/@susan_wilkinson?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
