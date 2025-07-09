+++
author = "FlintyLemming"
title = "【归档】iOS 12 futurerestore"
slug = "356ba7f667a847ff8b20768484f61e27"
date = "2020-06-01"
description = ""
categories = ["Apple"]
tags = ["iOS", "Jailbreak"]
image = "https://img.mitsea.com/blog/posts/2020/06/iOS%2012%20futurerestore/title.avif"
+++

请确保你保存有对应版本的 blobs/shsh 文件，如果你没保存或者不知道这是什么，请关闭页面

## 准备材料

1. unc0ver 越狱工具

    项目地址：https://github.com/pwn20wndstuff/Undecimus
2. 需要刷入的固件

    下载地址：ipsw.me
3. futurerestore 工具

    项目地址：https://github.com/s0uthwest/futurerestore
4. Cydia Impactor

    下载地址：http://www.cydiaimpactor.com

5. blobs

## 具体步骤
### 固定G值
1. 找到下载的 shsh 文件，可能有好几个，挑一个最新的，用文本编辑器打开它

    ![](https://img.mitsea.com/blog/posts/2020/06/iOS%2012%20futurerestore/1.avif)
2. 搜索“gen”，定位到 generator 值，下面那一串就是我们需要的G值

    ![](https://img.mitsea.com/blog/posts/2020/06/iOS%2012%20futurerestore/2.avif)
3. 参照这篇文章的教程，安装 unc0ver app

    ![](https://img.mitsea.com/blog/posts/2020/06/iOS%2012%20futurerestore/3.avif)
4. 在 Settings 里找到 Boot Nonce，把刚刚找到的G值填写进去

    ![](https://img.mitsea.com/blog/posts/2020/06/iOS%2012%20futurerestore/4.avif)
5. 回到 Jailbreak 页面，点击 Jailbreak
    > 这里如果重启，则表示失败，请重试。如果多次失败，开机后不要立即越狱，先操作一会再使用越狱工具
6. 越狱成功后，会提示我们 boot nonce 已经被覆写

    ![](https://img.mitsea.com/blog/posts/2020/06/iOS%2012%20futurerestore/5.avif)
7. 点击 Ok 后 app 会退出，我们手动重启设备，或者回到 app 里，有个重启按钮
8. 重启后，重新进入 unc0ver app，进入 setting，检查 Boot Nonce 值是不是我们刚才写入的值。如果不是的了，那就要重复前面的步骤重写，重新越狱。如果还是刚才写的值，说明G值固定成功。

### 准备文件
**我们一共需要准备六个文件**
#### 系统固件
这个不再赘述
#### shsh 文件
按照上面说的文件目录，挑一个最新的
#### futurerestore 工具
一个可执行文件

![](https://img.mitsea.com/blog/posts/2020/06/iOS%2012%20futurerestore/6.avif)
#### SEP 文件
1. 解压下载的系统固件
2. 在 Firmware/all_flash 目录下，有一堆以 “sep” 开头的文件，但是它有很多种，比如我这里有 j120、j121 等
3. 查看刚才保存的 shsh 文件的文件名，比如我这里就可以看到是 j207，那我就选 “sep-firmware.j207.RELEASE.im4p” 这个文件，注意不是 plist 格式的那个

    ![](https://img.mitsea.com/blog/posts/2020/06/iOS%2012%20futurerestore/7.avif)
#### 基带固件
在解压缩的系统固件的 Firmware 文件夹里，通常还有一些 .bbfw 格式的文件，这些是基带文件。这就需要你查一下你手机对应的是哪个基带文件了，我这里是 WLAN 版 iPad，所以只有一个



#### BuildManifest.plist 文件
解压缩系统固件，在根目录就能看到这个文件

![](https://img.mitsea.com/blog/posts/2020/06/iOS%2012%20futurerestore/8.avif)

**这样我们就准备好了六个文件**
![](https://img.mitsea.com/blog/posts/2020/06/iOS%2012%20futurerestore/9.avif)

### 开始刷机
1. 打开 终端 app
2. 输入 <futurerestore 文件路径> -t <shsh 文件路径> -s <sep 文件路径> -b <基带文件路径> -p <BuildManifest.plist 文件路径> -m <BuildManifest.plist 文件路径> -d <系统固件路径>

    ![](https://img.mitsea.com/blog/posts/2020/06/iOS%2012%20futurerestore/10.avif)
3. 按回车设备就会自动进入restore模式并开始刷机了，如果出现错误，按照提示进行搜索处理