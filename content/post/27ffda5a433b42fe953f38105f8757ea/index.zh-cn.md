+++
author = "FlintyLemming"
title = "共用账号场景下的管理多人 iOS 开发和发布全流程"
slug = "27ffda5a433b42fe953f38105f8757ea"
date = "2023-03-02"
description = "也算是梳理了一下 App 上架的部分过程"
categories = ["Coding"]
tags = ["Apple", "Develop"]
image = "https://img.mitsea.com/blog/posts/2023/03/%E5%85%B1%E7%94%A8%E8%B4%A6%E5%8F%B7%E5%9C%BA%E6%99%AF%E4%B8%8B%E7%9A%84%E7%AE%A1%E7%90%86%E5%A4%9A%E4%BA%BA%20iOS%20%E5%BC%80%E5%8F%91%E5%92%8C%E5%8F%91%E5%B8%83%E5%85%A8%E6%B5%81%E7%A8%8B/y-s-cIED8mVpyTY-unsplash.avif"
+++

虽然按理说应该是每个开发者一个 Apple ID，然后拉到组织内统一管理，但是在实际开发中有时也会采用共用同一个人的签名进行开发的方式。比如这个项目可能是外包的，别人并不会给你弄一个账号。

不过事先说明下，这个管理方法属于野路子，仅限内部小规模开发用。因为其中生成的证书需要安装到其他开发人员的机器上，这个证书是可以被随意导出的。

## 收集设备信息

需要收集开发所使用的 iOS 设备的 UDID

iOS 设备的 UDID 需要将其连接至 Mac，在 Finder 左侧找到设备，点击上方显示设备文件名和容量的这个位置

![](https://img.mitsea.com/blog/posts/2023/03/%E5%85%B1%E7%94%A8%E8%B4%A6%E5%8F%B7%E5%9C%BA%E6%99%AF%E4%B8%8B%E7%9A%84%E7%AE%A1%E7%90%86%E5%A4%9A%E4%BA%BA%20iOS%20%E5%BC%80%E5%8F%91%E5%92%8C%E5%8F%91%E5%B8%83%E5%85%A8%E6%B5%81%E7%A8%8B/Untitled.avif)

这里就会显示 UDID

![](https://img.mitsea.com/blog/posts/2023/03/%E5%85%B1%E7%94%A8%E8%B4%A6%E5%8F%B7%E5%9C%BA%E6%99%AF%E4%B8%8B%E7%9A%84%E7%AE%A1%E7%90%86%E5%A4%9A%E4%BA%BA%20iOS%20%E5%BC%80%E5%8F%91%E5%92%8C%E5%8F%91%E5%B8%83%E5%85%A8%E6%B5%81%E7%A8%8B/Untitled%201.avif)

收集完毕后，在 ****Certificates, Identifiers & Profiles**** 的 Devices 页面里把所有设备都注册上

![](https://img.mitsea.com/blog/posts/2023/03/%E5%85%B1%E7%94%A8%E8%B4%A6%E5%8F%B7%E5%9C%BA%E6%99%AF%E4%B8%8B%E7%9A%84%E7%AE%A1%E7%90%86%E5%A4%9A%E4%BA%BA%20iOS%20%E5%BC%80%E5%8F%91%E5%92%8C%E5%8F%91%E5%B8%83%E5%85%A8%E6%B5%81%E7%A8%8B/Untitled%202.avif)

## 创建 ****Certificates（证书）****

1. 当开始创建 Certificates 的时候，会让你选择类别，需要创建两个，一个 Apple Development、一个 Apple Distribution。图上 Apple Development 选不了是因为只能创建一个，我创建过了。

    ![](https://img.mitsea.com/blog/posts/2023/03/%E5%85%B1%E7%94%A8%E8%B4%A6%E5%8F%B7%E5%9C%BA%E6%99%AF%E4%B8%8B%E7%9A%84%E7%AE%A1%E7%90%86%E5%A4%9A%E4%BA%BA%20iOS%20%E5%BC%80%E5%8F%91%E5%92%8C%E5%8F%91%E5%B8%83%E5%85%A8%E6%B5%81%E7%A8%8B/Untitled%203.avif)

    创建过程很简单，都有文档提示，大致就是需要本地先生成一个请求密钥的文件，然后上传上去，就会给你下发证书。两个都弄完，你会分别获得 development.cer 和 distribution.cer 两个文件。

    ![](https://img.mitsea.com/blog/posts/2023/03/%E5%85%B1%E7%94%A8%E8%B4%A6%E5%8F%B7%E5%9C%BA%E6%99%AF%E4%B8%8B%E7%9A%84%E7%AE%A1%E7%90%86%E5%A4%9A%E4%BA%BA%20iOS%20%E5%BC%80%E5%8F%91%E5%92%8C%E5%8F%91%E5%B8%83%E5%85%A8%E6%B5%81%E7%A8%8B/Untitled%204.avif)

2. 双击这两个文件，把证书安装到钥匙串里

    ![](https://img.mitsea.com/blog/posts/2023/03/%E5%85%B1%E7%94%A8%E8%B4%A6%E5%8F%B7%E5%9C%BA%E6%99%AF%E4%B8%8B%E7%9A%84%E7%AE%A1%E7%90%86%E5%A4%9A%E4%BA%BA%20iOS%20%E5%BC%80%E5%8F%91%E5%92%8C%E5%8F%91%E5%B8%83%E5%85%A8%E6%B5%81%E7%A8%8B/Untitled%205.avif)

3. 安装后，这两个 cer 文件就没用了。我们需要把这个证书导出为 .p12 文件，右键刚才安装的证书，选择导出，设置一个密码，就得到了两个 p12 文件。

    ![](https://img.mitsea.com/blog/posts/2023/03/%E5%85%B1%E7%94%A8%E8%B4%A6%E5%8F%B7%E5%9C%BA%E6%99%AF%E4%B8%8B%E7%9A%84%E7%AE%A1%E7%90%86%E5%A4%9A%E4%BA%BA%20iOS%20%E5%BC%80%E5%8F%91%E5%92%8C%E5%8F%91%E5%B8%83%E5%85%A8%E6%B5%81%E7%A8%8B/Untitled%206.avif)

    这个 p12 文件和你设置的密码一定不能丢！！！后面为了业务服务的描述文件都会依赖这个证书。如果是公司的，你千万不能泄露，市面上泄露的所谓的企业签名，就是因为泄露了这个证书。

    ![](https://img.mitsea.com/blog/posts/2023/03/%E5%85%B1%E7%94%A8%E8%B4%A6%E5%8F%B7%E5%9C%BA%E6%99%AF%E4%B8%8B%E7%9A%84%E7%AE%A1%E7%90%86%E5%A4%9A%E4%BA%BA%20iOS%20%E5%BC%80%E5%8F%91%E5%92%8C%E5%8F%91%E5%B8%83%E5%85%A8%E6%B5%81%E7%A8%8B/Untitled%207.avif)

4. 把这个 p12 证书导入到所有开发用 Mac 的钥匙串里

## 创建 App ID

这一步主要是要给你的 App 设定一个包名。下图中，Description 随便写，他不是商店显示的名称，也不是 App Icon 下的名称；Bundle ID 就是包名；Capabilities 选择你需要使用的功能，不知道选什么也没关系，这个后面可以改。

![](https://img.mitsea.com/blog/posts/2023/03/%E5%85%B1%E7%94%A8%E8%B4%A6%E5%8F%B7%E5%9C%BA%E6%99%AF%E4%B8%8B%E7%9A%84%E7%AE%A1%E7%90%86%E5%A4%9A%E4%BA%BA%20iOS%20%E5%BC%80%E5%8F%91%E5%92%8C%E5%8F%91%E5%B8%83%E5%85%A8%E6%B5%81%E7%A8%8B/Untitled%208.avif)

## 创建开发用的 Profiles （描述文件）并开发

这一步就是生成如果你不勾选自动签名，这里需要的 Provisioning Profile

![](https://img.mitsea.com/blog/posts/2023/03/%E5%85%B1%E7%94%A8%E8%B4%A6%E5%8F%B7%E5%9C%BA%E6%99%AF%E4%B8%8B%E7%9A%84%E7%AE%A1%E7%90%86%E5%A4%9A%E4%BA%BA%20iOS%20%E5%BC%80%E5%8F%91%E5%92%8C%E5%8F%91%E5%B8%83%E5%85%A8%E6%B5%81%E7%A8%8B/Untitled%209.avif)

1. 创建 Profile 的时候，选择 iOS App Development

    ![](https://img.mitsea.com/blog/posts/2023/03/%E5%85%B1%E7%94%A8%E8%B4%A6%E5%8F%B7%E5%9C%BA%E6%99%AF%E4%B8%8B%E7%9A%84%E7%AE%A1%E7%90%86%E5%A4%9A%E4%BA%BA%20iOS%20%E5%BC%80%E5%8F%91%E5%92%8C%E5%8F%91%E5%B8%83%E5%85%A8%E6%B5%81%E7%A8%8B/Untitled%2010.avif)

2. 这里选第四步创建的 App ID

    ![](https://img.mitsea.com/blog/posts/2023/03/%E5%85%B1%E7%94%A8%E8%B4%A6%E5%8F%B7%E5%9C%BA%E6%99%AF%E4%B8%8B%E7%9A%84%E7%AE%A1%E7%90%86%E5%A4%9A%E4%BA%BA%20iOS%20%E5%BC%80%E5%8F%91%E5%92%8C%E5%8F%91%E5%B8%83%E5%85%A8%E6%B5%81%E7%A8%8B/Untitled%2011.avif)

3. 这里选择第二步选择的证书

    ![](https://img.mitsea.com/blog/posts/2023/03/%E5%85%B1%E7%94%A8%E8%B4%A6%E5%8F%B7%E5%9C%BA%E6%99%AF%E4%B8%8B%E7%9A%84%E7%AE%A1%E7%90%86%E5%A4%9A%E4%BA%BA%20iOS%20%E5%BC%80%E5%8F%91%E5%92%8C%E5%8F%91%E5%B8%83%E5%85%A8%E6%B5%81%E7%A8%8B/Untitled%2012.avif)

4. 这里选择第一步添加的设备

    ![](https://img.mitsea.com/blog/posts/2023/03/%E5%85%B1%E7%94%A8%E8%B4%A6%E5%8F%B7%E5%9C%BA%E6%99%AF%E4%B8%8B%E7%9A%84%E7%AE%A1%E7%90%86%E5%A4%9A%E4%BA%BA%20iOS%20%E5%BC%80%E5%8F%91%E5%92%8C%E5%8F%91%E5%B8%83%E5%85%A8%E6%B5%81%E7%A8%8B/Untitled%2013.avif)

5. 写一个名字后导出，会得到一个 .mobileprovision 的文件
6. Xcode 里 Provisioning Profile 位置导入这个文件后，就可以使用 Xcode 在注册的机器上进行真机调试了

    ![](https://img.mitsea.com/blog/posts/2023/03/%E5%85%B1%E7%94%A8%E8%B4%A6%E5%8F%B7%E5%9C%BA%E6%99%AF%E4%B8%8B%E7%9A%84%E7%AE%A1%E7%90%86%E5%A4%9A%E4%BA%BA%20iOS%20%E5%BC%80%E5%8F%91%E5%92%8C%E5%8F%91%E5%B8%83%E5%85%A8%E6%B5%81%E7%A8%8B/Untitled%2014.avif)

## 在 App Store Connect 里为 App 创建坑位

从下面这个网址进入 App Store Connect 的 App 管理页面并新建 App

[https://appstoreconnect.apple.com/apps](https://appstoreconnect.apple.com/apps)

名称是 App Store 里显示的名称；套装 ID 就是包名；SKU 一般填包名就行

![](https://img.mitsea.com/blog/posts/2023/03/%E5%85%B1%E7%94%A8%E8%B4%A6%E5%8F%B7%E5%9C%BA%E6%99%AF%E4%B8%8B%E7%9A%84%E7%AE%A1%E7%90%86%E5%A4%9A%E4%BA%BA%20iOS%20%E5%BC%80%E5%8F%91%E5%92%8C%E5%8F%91%E5%B8%83%E5%85%A8%E6%B5%81%E7%A8%8B/Untitled%2015.avif)

## 创建发布用的 Profiles （描述文件）并上传至 Connect

这一步跟第四步差不多，就是选择类别的时候选 App Store

![](https://img.mitsea.com/blog/posts/2023/03/%E5%85%B1%E7%94%A8%E8%B4%A6%E5%8F%B7%E5%9C%BA%E6%99%AF%E4%B8%8B%E7%9A%84%E7%AE%A1%E7%90%86%E5%A4%9A%E4%BA%BA%20iOS%20%E5%BC%80%E5%8F%91%E5%92%8C%E5%8F%91%E5%B8%83%E5%85%A8%E6%B5%81%E7%A8%8B/Untitled%2016.avif)

💡 如果不是发布到商店，而是分发给几个特定设备先试用，选 Ad Hoc

然后同样下载到一份描述文件，这个是用于发布的。下面说一下发布流程

1. Xcode 里 Product - Archive

    ![](https://img.mitsea.com/blog/posts/2023/03/%E5%85%B1%E7%94%A8%E8%B4%A6%E5%8F%B7%E5%9C%BA%E6%99%AF%E4%B8%8B%E7%9A%84%E7%AE%A1%E7%90%86%E5%A4%9A%E4%BA%BA%20iOS%20%E5%BC%80%E5%8F%91%E5%92%8C%E5%8F%91%E5%B8%83%E5%85%A8%E6%B5%81%E7%A8%8B/Untitled%2017.avif)

2. Distribute App

    ![](https://img.mitsea.com/blog/posts/2023/03/%E5%85%B1%E7%94%A8%E8%B4%A6%E5%8F%B7%E5%9C%BA%E6%99%AF%E4%B8%8B%E7%9A%84%E7%AE%A1%E7%90%86%E5%A4%9A%E4%BA%BA%20iOS%20%E5%BC%80%E5%8F%91%E5%92%8C%E5%8F%91%E5%B8%83%E5%85%A8%E6%B5%81%E7%A8%8B/Untitled%2018.avif)

3. 选择第一个，如果前面选了 Ad Hoc，这里选第二个

    ![](https://img.mitsea.com/blog/posts/2023/03/%E5%85%B1%E7%94%A8%E8%B4%A6%E5%8F%B7%E5%9C%BA%E6%99%AF%E4%B8%8B%E7%9A%84%E7%AE%A1%E7%90%86%E5%A4%9A%E4%BA%BA%20iOS%20%E5%BC%80%E5%8F%91%E5%92%8C%E5%8F%91%E5%B8%83%E5%85%A8%E6%B5%81%E7%A8%8B/Untitled%2019.avif)

4. Upload。如果不需要发布，比如说你是接活的，人家不需要你来操作，那就 Export。

    ![](https://img.mitsea.com/blog/posts/2023/03/%E5%85%B1%E7%94%A8%E8%B4%A6%E5%8F%B7%E5%9C%BA%E6%99%AF%E4%B8%8B%E7%9A%84%E7%AE%A1%E7%90%86%E5%A4%9A%E4%BA%BA%20iOS%20%E5%BC%80%E5%8F%91%E5%92%8C%E5%8F%91%E5%B8%83%E5%85%A8%E6%B5%81%E7%A8%8B/Untitled%2020.avif)

5. 全选

    ![](https://img.mitsea.com/blog/posts/2023/03/%E5%85%B1%E7%94%A8%E8%B4%A6%E5%8F%B7%E5%9C%BA%E6%99%AF%E4%B8%8B%E7%9A%84%E7%AE%A1%E7%90%86%E5%A4%9A%E4%BA%BA%20iOS%20%E5%BC%80%E5%8F%91%E5%92%8C%E5%8F%91%E5%B8%83%E5%85%A8%E6%B5%81%E7%A8%8B/Untitled%2021.avif)

6. 这里就要注意了，Distribution certificate 是选第2步创建的证书，要选择那个 Distribution 类型的。下面的要选刚才下载到的描述文件。

    ![](https://img.mitsea.com/blog/posts/2023/03/%E5%85%B1%E7%94%A8%E8%B4%A6%E5%8F%B7%E5%9C%BA%E6%99%AF%E4%B8%8B%E7%9A%84%E7%AE%A1%E7%90%86%E5%A4%9A%E4%BA%BA%20iOS%20%E5%BC%80%E5%8F%91%E5%92%8C%E5%8F%91%E5%B8%83%E5%85%A8%E6%B5%81%E7%A8%8B/Untitled%2022.avif)

7. 最后没问题就 Upload，他会直接给你上传到 AppStore Connect 里。如果你之前选的是 Export，别人拿到后解压里面有个 .ipa 文件，他用 Transporter 也可以上传到 AppStore Connect 里。

    [Transporter](https://apps.apple.com/cn/app/transporter/id1450874784?mt=12)

8. 上传完后，登录 AppStore Connect 里就能看到

    发布商店的话，就在这里选版本

    ![](https://img.mitsea.com/blog/posts/2023/03/%E5%85%B1%E7%94%A8%E8%B4%A6%E5%8F%B7%E5%9C%BA%E6%99%AF%E4%B8%8B%E7%9A%84%E7%AE%A1%E7%90%86%E5%A4%9A%E4%BA%BA%20iOS%20%E5%BC%80%E5%8F%91%E5%92%8C%E5%8F%91%E5%B8%83%E5%85%A8%E6%B5%81%E7%A8%8B/Untitled%2023.avif)

    TestFlight 也类似，不过 TestFlight 需要先创建一个测试组。内部测试的话，不需要审核，但是需要把所有测试者拉到你的组织下。外部测试需要审核。

    ![](https://img.mitsea.com/blog/posts/2023/03/%E5%85%B1%E7%94%A8%E8%B4%A6%E5%8F%B7%E5%9C%BA%E6%99%AF%E4%B8%8B%E7%9A%84%E7%AE%A1%E7%90%86%E5%A4%9A%E4%BA%BA%20iOS%20%E5%BC%80%E5%8F%91%E5%92%8C%E5%8F%91%E5%B8%83%E5%85%A8%E6%B5%81%E7%A8%8B/Untitled%2024.avif)

> Photo by [Y S](https://unsplash.com/@santonii?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
  