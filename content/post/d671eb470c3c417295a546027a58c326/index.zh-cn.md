+++
author = "FlintyLemming"
title = "【归档】iOS 如何安装多个相同应用"
slug = "d671eb470c3c417295a546027a58c326"
date = "2019-10-11"
description = ""
categories = ["Apple"]
tags = ["iOS"]
image = "https://hf-image.mitsea.com:8840/blog/posts/2019/10/iOS%20%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%85%E5%A4%9A%E4%B8%AA%E7%9B%B8%E5%90%8C%E5%BA%94%E7%94%A8/title.avif"
+++

## 提要
本教程在修订后已经适合一般用户阅读和使用，但仍需要一些基础的计算机基础以及最重要的耐心

教程中包含个别付费工具，但完全可以不用，整个过程是免费的。不过对于没有越狱，并且没有开发者订阅资格的用户，需要每周重新操作一次（很小的一部分，但需要电脑，三五分钟即可完成）

## 1 准备 ipa 安装包
### 1.1 未越狱设备
如果你的设备没有越狱，你需要准备一个脱壳的 ipa 安装包，即不含账号信息的 ipa 安装包，获取方法有两种。
#### 1.1.1 热门软件
你可以在一些手机助手软件内直接获取，下面我以 PP助手 为例
> 爱思助手我看了，没有
1. 下载并安装 PP助手
2. 不需要连接设备，如果提示缺少驱动，无视，关闭窗口即可
    > 尤其是如果你安装的是 Windows 商店版本的 iTunes，很多助手工具不认它的驱动
3. 打开 找应用-越狱应用

    ![](https://hf-image.mitsea.com:8840/blog/posts/2019/10/iOS%20%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%85%E5%A4%9A%E4%B8%AA%E7%9B%B8%E5%90%8C%E5%BA%94%E7%94%A8/1.avif)

4. 这里以 小米计算器 为例，搜索并下载

    ![](https://hf-image.mitsea.com:8840/blog/posts/2019/10/iOS%20%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%85%E5%A4%9A%E4%B8%AA%E7%9B%B8%E5%90%8C%E5%BA%94%E7%94%A8/2.avif)

5. 点击右上角的下载管理并打开下载文件夹，可以看到获得一个“越狱版”的ipa，这就是一个脱壳的ipa，下载好备用，如果没有其他需要，请移步 2 修改 ipa 文件

    ![](https://hf-image.mitsea.com:8840/blog/posts/2019/10/iOS%20%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%85%E5%A4%9A%E4%B8%AA%E7%9B%B8%E5%90%8C%E5%BA%94%E7%94%A8/3.avif)

#### 1.1.2 冷门软件
如果很不幸，你的软件没有被 PP助手 这样的助手工具里的越狱商店收录，那你只能依靠一台已经越狱的设备通过砸壳工具来获得脱壳 ipa
1. 在越狱商店里下载 CrackerXI 这个插件，作者官方源我还不知道是哪一个，反正国内各大盗版源都有，直接搜就好。

    ![](https://hf-image.mitsea.com:8840/blog/posts/2019/10/iOS%20%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%85%E5%A4%9A%E4%B8%AA%E7%9B%B8%E5%90%8C%E5%BA%94%E7%94%A8/4.avif)

2. 安装好后打开 CrackerXI，打开设置，打开 CrackerXI Hook 开关

    ![](https://hf-image.mitsea.com:8840/blog/posts/2019/10/iOS%20%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%85%E5%A4%9A%E4%B8%AA%E7%9B%B8%E5%90%8C%E5%BA%94%E7%94%A8/5.avif)

3. 回到 App 列表，找到小米计算器，点击后一路确定，稍等一会，就能获取到脱壳的ipa文件，按照给的路径在Filza里找到它

    ![](https://hf-image.mitsea.com:8840/blog/posts/2019/10/iOS%20%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%85%E5%A4%9A%E4%B8%AA%E7%9B%B8%E5%90%8C%E5%BA%94%E7%94%A8/6.avif)

    ![](https://hf-image.mitsea.com:8840/blog/posts/2019/10/iOS%20%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%85%E5%A4%9A%E4%B8%AA%E7%9B%B8%E5%90%8C%E5%BA%94%E7%94%A8/7.avif)

### 1.2 已越狱设备
如果你的设备已经越狱，那只需要准备一个需要多开的 app 的 ipa 安装包就行，即使包含有你的账号信息。那么除了 1.1 中介绍的方法，这里还有两种方法。
#### 1.2.1 不用电脑（可行性存疑）
**AppStore 对于抓取 ipa 做了限制，目前可能无法通过这种方法获取 ipa 文件**
在iOS上获取 ipa 文件需要抓包软件，关于如何通过抓包软件获取ipa文件，可以参考这篇教程[如何抓取 iOS app 的安装包](https://www.flinty.moe/how-to-get-ipa/)，抓包软件是需要钱的。
#### 1.2.2 需要电脑
1. 电脑上搜索并安装 iMazing 这款软件，由于新版本的 iTunes 无法下载 ipa 文件，也不建议换老版本，所以用 iMazing 下载 ipa。
2. 打开 iMazing，连接上电脑，看到这样的界面。

    ![](https://hf-image.mitsea.com:8840/blog/posts/2019/10/iOS%20%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%85%E5%A4%9A%E4%B8%AA%E7%9B%B8%E5%90%8C%E5%BA%94%E7%94%A8/8.avif)

3. 点击“管理应用程序”

    ![](https://hf-image.mitsea.com:8840/blog/posts/2019/10/iOS%20%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%85%E5%A4%9A%E4%B8%AA%E7%9B%B8%E5%90%8C%E5%BA%94%E7%94%A8/9.avif)

4. 稍等片刻，就可以查看 Apple ID 资料库中的 app，并且请确认右上角是否登陆有 Apple ID，如果没有，请登陆

    ![](https://hf-image.mitsea.com:8840/blog/posts/2019/10/iOS%20%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%85%E5%A4%9A%E4%B8%AA%E7%9B%B8%E5%90%8C%E5%BA%94%E7%94%A8/10.avif)

5. 在列表里搜索你需要多开的app，比如我这里想装两个小米计算器（演示用，小文件方便），就点击右边的下载图标，这里就下载好了

    ![](https://hf-image.mitsea.com:8840/blog/posts/2019/10/iOS%20%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%85%E5%A4%9A%E4%B8%AA%E7%9B%B8%E5%90%8C%E5%BA%94%E7%94%A8/11.avif)

6. 右键小米计算器这一项，点击导出 ipa

    ![](https://hf-image.mitsea.com:8840/blog/posts/2019/10/iOS%20%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%85%E5%A4%9A%E4%B8%AA%E7%9B%B8%E5%90%8C%E5%BA%94%E7%94%A8/12.avif)

## 2 修改 ipa 安装包
显然同一个设备不可以安装包名相同的两个应用，所以我们需要修改获取的 ipa 文件的包名
### 2.1 macOS 用户
1. 先解压ipa，用系统自带的解压缩工具就行

    ![](https://hf-image.mitsea.com:8840/blog/posts/2019/10/iOS%20%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%85%E5%A4%9A%E4%B8%AA%E7%9B%B8%E5%90%8C%E5%BA%94%E7%94%A8/13.avif)

2. 打开解压出来的文件夹，在 payload 文件夹下找到app本体，右键-显示包内容

    ![](https://hf-image.mitsea.com:8840/blog/posts/2019/10/iOS%20%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%85%E5%A4%9A%E4%B8%AA%E7%9B%B8%E5%90%8C%E5%BA%94%E7%94%A8/14.avif)

3. 找到 info.plist 这个文件，使用 Xcode 打开，如果没有安装 Xcode，用文本编辑器打开，都可以，后面会讲如何在文本编辑器里编辑

    ![](https://hf-image.mitsea.com:8840/blog/posts/2019/10/iOS%20%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%85%E5%A4%9A%E4%B8%AA%E7%9B%B8%E5%90%8C%E5%BA%94%E7%94%A8/15.avif)

4. 找到 Bundle identifier 这一项，修改后面的内容，只要跟原来不同就行，比如我这里直接在后面加几个字。

    ![](https://hf-image.mitsea.com:8840/blog/posts/2019/10/iOS%20%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%85%E5%A4%9A%E4%B8%AA%E7%9B%B8%E5%90%8C%E5%BA%94%E7%94%A8/16.avif)

    如果你是用文本编辑器打开的，直接搜索CFBundleIdentifier，也可以找到这一项，修改后面的内容即可。

    ![](https://hf-image.mitsea.com:8840/blog/posts/2019/10/iOS%20%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%85%E5%A4%9A%E4%B8%AA%E7%9B%B8%E5%90%8C%E5%BA%94%E7%94%A8/17.avif)

5. 改好之后保存，关闭编辑器，回到这里，全选文件，压缩

    ![](https://hf-image.mitsea.com:8840/blog/posts/2019/10/iOS%20%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%85%E5%A4%9A%E4%B8%AA%E7%9B%B8%E5%90%8C%E5%BA%94%E7%94%A8/18.avif)

6. 压缩后获得一个 归档.zip 的文件，修改后缀为 .ipa，这里选择“使用 .ipa”

    ![](https://hf-image.mitsea.com:8840/blog/posts/2019/10/iOS%20%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%85%E5%A4%9A%E4%B8%AA%E7%9B%B8%E5%90%8C%E5%BA%94%E7%94%A8/19.avif)

7. 然后就获得了与官方包名不同的 ipa 文件

    ![](https://hf-image.mitsea.com:8840/blog/posts/2019/10/iOS%20%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%85%E5%A4%9A%E4%B8%AA%E7%9B%B8%E5%90%8C%E5%BA%94%E7%94%A8/20.avif)

### 2.2 Windows 用户
1. 解压下载得到的 ipa，如果不会直接解压，修改后缀为 zip 再解压

    ![](https://hf-image.mitsea.com:8840/blog/posts/2019/10/iOS%20%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%85%E5%A4%9A%E4%B8%AA%E7%9B%B8%E5%90%8C%E5%BA%94%E7%94%A8/21.avif)

2. 进入到 `小米计算器-1.0.3(越狱应用)\Payload\Calculator.app` 目录下，找到 info.plist 文件

    ![](https://hf-image.mitsea.com:8840/blog/posts/2019/10/iOS%20%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%85%E5%A4%9A%E4%B8%AA%E7%9B%B8%E5%90%8C%E5%BA%94%E7%94%A8/22.avif)

3. 如果没有安装其他的编辑器，使用记事本打开

    ![](https://hf-image.mitsea.com:8840/blog/posts/2019/10/iOS%20%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%85%E5%A4%9A%E4%B8%AA%E7%9B%B8%E5%90%8C%E5%BA%94%E7%94%A8/23.avif)

4. 找到 CFBundleIdentifier 这个键值，修改后面的内容，比如这里是 com.xiaomi.xiaomicalculator，你可以改成 com.xiaomi.xiaomicalculatortest 

    ![](https://hf-image.mitsea.com:8840/blog/posts/2019/10/iOS%20%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%85%E5%A4%9A%E4%B8%AA%E7%9B%B8%E5%90%8C%E5%BA%94%E7%94%A8/24.avif)

5. 保存，回到 `小米计算器-1.0.3(越狱应用)\` 目录下，将这里的所有文件打包成 zip，并改后缀为 ipa

    ![](https://hf-image.mitsea.com:8840/blog/posts/2019/10/iOS%20%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%85%E5%A4%9A%E4%B8%AA%E7%9B%B8%E5%90%8C%E5%BA%94%E7%94%A8/25.avif)

### 2.3 在设备上修改
这里就不赘述了，越狱用户其实可以全程手机上操作，Filza 就可以直接修改，Shu 也可以

## 3 安装
### 3.1 未越狱设备
除非有开发者账号，否则目前没有什么特别好的安装方法。

### 3.2 已越狱设备
1. 通过 iTunes 或者其他工具把 ipa 文件拷贝到某个 app 的文档下，比如我这里拷贝到 Shu 这个 app 里。
    ![](https://hf-image.mitsea.com:8840/blog/posts/2019/10/iOS%20%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%85%E5%A4%9A%E4%B8%AA%E7%9B%B8%E5%90%8C%E5%BA%94%E7%94%A8/26.avif)
2. 设备上安装 Filza 这款软件，在越狱商店里（Cydia 或者 Sileo）直接搜索，BigBoss 源里就有。
    ![](https://hf-image.mitsea.com:8840/blog/posts/2019/10/iOS%20%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%85%E5%A4%9A%E4%B8%AA%E7%9B%B8%E5%90%8C%E5%BA%94%E7%94%A8/27.avif)
3. 打开 Filza，左边点开 App管理器，右边找到刚才放进去的app，比如我用的 Shu，那就打开Shu - Documents，找到放进去的 ipa文件，点安装。
    ![](https://hf-image.mitsea.com:8840/blog/posts/2019/10/iOS%20%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%85%E5%A4%9A%E4%B8%AA%E7%9B%B8%E5%90%8C%E5%BA%94%E7%94%A8/28.avif)
4. 稍等片刻，就安装完了，可以看到有两个小米计算器
    ![](https://hf-image.mitsea.com:8840/blog/posts/2019/10/iOS%20%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%85%E5%A4%9A%E4%B8%AA%E7%9B%B8%E5%90%8C%E5%BA%94%E7%94%A8/29.avif)
