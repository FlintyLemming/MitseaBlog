+++
author = "FlintyLemming"
title = "iOS 12 rootless Jailbreak 越狱以及手动安装插件"
slug = "e0501afac71f49d787cf8e74ed656358"
date = "2020-02-01"
description = ""
categories = ["Apple"]
tags = ["iOS", "Jailbreak"]
image = "https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2020/02/iOS%2012%20rootless%20Jailbreak%20%E8%B6%8A%E7%8B%B1%E4%BB%A5%E5%8F%8A%E6%89%8B%E5%8A%A8%E5%AE%89%E8%A3%85%E6%8F%92%E4%BB%B6/title.avif"
+++

## 安装文件管理器
这里我用的是 GeoFilza，项目地址是：https://github.com/GeoSn0w/GeoFilza
1. 在项目页面依次点击 Clone and Download - Download ZIP
2. 解压下载的压缩包，获得 GeoFilza.ipa 文件
3. 参考[这篇教程](https://www.flinty.moe/cydia-impactor/)，安装 GeoFilza
4. 启动会比较缓慢，有个破解的过程，如果失败造成重启，再试一次

## 越狱系统
**由于越狱工具仅通过ipa安装会不起作用，所以只能通过 xcode 编译安装，如果你没有 macOS，请放弃以下操作，或者虚拟机安装 macOS。如果该工具后续更新支持 ipa 直装，则可继续操作**
1. 从[项目地址](https://github.com/jakeajames/rootlessJB3)下载项目文件

2. 从 AppStore 下载安装 xcode

3. 解压下载的项目文件，双击 rootlessJB.xcodeproj 即可快速加载项目。如果是第一次使用 xcode，可能会需要同意一些使用条款

    ![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2020/02/iOS%2012%20rootless%20Jailbreak%20%E8%B6%8A%E7%8B%B1%E4%BB%A5%E5%8F%8A%E6%89%8B%E5%8A%A8%E5%AE%89%E8%A3%85%E6%8F%92%E4%BB%B6/1.avif)

    这里点击 Open

    ![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2020/02/iOS%2012%20rootless%20Jailbreak%20%E8%B6%8A%E7%8B%B1%E4%BB%A5%E5%8F%8A%E6%89%8B%E5%8A%A8%E5%AE%89%E8%A3%85%E6%8F%92%E4%BB%B6/2.avif)

4. 打开后默认是这样的界面

    ![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2020/02/iOS%2012%20rootless%20Jailbreak%20%E8%B6%8A%E7%8B%B1%E4%BB%A5%E5%8F%8A%E6%89%8B%E5%8A%A8%E5%AE%89%E8%A3%85%E6%8F%92%E4%BB%B6/3.avif)

    如果不是，双击这里

    ![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2020/02/iOS%2012%20rootless%20Jailbreak%20%E8%B6%8A%E7%8B%B1%E4%BB%A5%E5%8F%8A%E6%89%8B%E5%8A%A8%E5%AE%89%E8%A3%85%E6%8F%92%E4%BB%B6/4.avif)

5. 修改 Bundle Indentifier 包名，比如原来是
    ```
    com.jakeashacks.rootlessJB3
    ```
    可以改为
    ```
    test.jakeashacks.rootlessJB3
    ```
6. 在 Team 里添加账号

    ![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2020/02/iOS%2012%20rootless%20Jailbreak%20%E8%B6%8A%E7%8B%B1%E4%BB%A5%E5%8F%8A%E6%89%8B%E5%8A%A8%E5%AE%89%E8%A3%85%E6%8F%92%E4%BB%B6/5.avif)

7. 插上手机，左上角选择自己的设备，然后点击箭头编译安装

    ![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2020/02/iOS%2012%20rootless%20Jailbreak%20%E8%B6%8A%E7%8B%B1%E4%BB%A5%E5%8F%8A%E6%89%8B%E5%8A%A8%E5%AE%89%E8%A3%85%E6%8F%92%E4%BB%B6/6.avif)

8. 稍等片刻，设备上就会出现 rootlessJB 应用，验证后打开，点击 Jailbreak！ 即可越狱
    > 这里 iSuperSU 可选，类似一个任务管理器的工具。Tweaks 必选。
如果越狱后关机重启，则表明越狱失败，重试一遍。如果是 SpringBoard 重启，即有个小圈在转，则表明成功。

## 安装插件
### 仅有 xxx.dylib 和 xxx.plist 两个插件的工具
1. 如果你下载的工具是类似于这样的文件，那么安装起来比较简单

    ![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2020/02/iOS%2012%20rootless%20Jailbreak%20%E8%B6%8A%E7%8B%B1%E4%BB%A5%E5%8F%8A%E6%89%8B%E5%8A%A8%E5%AE%89%E8%A3%85%E6%8F%92%E4%BB%B6/7.avif)

2. 将这两个文件拷贝到手机目录里，比如说，你可以拷贝到某个文件管理app下，比如 Shu、Documents 这类

3. 在 GeoFilza 里找到这两个文件，应用文件目录一般在
    ```
    /var/mobile/Containers/Data/Application/<应用名>/Documents
    ```
4. 将这两个文件复制到
    ```
    /root/var/containers/Bundle/tweaksupport/Library/TweakInject
    ```
5. 重启手机，再打开 rootless 越狱工具重新越狱，插件则会生效

    如果不想重启，则用 SSH 连接手机，执行
    ```
    inject /var/LIB/MobileSubstrate/DynamicLibraries/插件名.dylib
    ```
    然后杀掉 SpringBoard 以重启它
    ```
    killall SpringBoard
    ```
### 有一些文件夹的工具
1. 如果你下载的工具有一些文件夹，像这样，他们通常包括设置内容，则需要依次复制几个文件

    ![](https://gitee.com/flintylemming/mitsea-public-source/raw/master/images/blog/posts/2020/02/iOS%2012%20rootless%20Jailbreak%20%E8%B6%8A%E7%8B%B1%E4%BB%A5%E5%8F%8A%E6%89%8B%E5%8A%A8%E5%AE%89%E8%A3%85%E6%8F%92%E4%BB%B6/8.avif)

2. 打开 GeoFilza ，定位到下述目录
    ```
    /var/LIB
    ```
    你就会发现同样有一些名为"MobileSubstrate"、"PreferenceLoader"、"PreferenceBundles"

3. 把下载的这些文件夹里的文件，分别复制到刚才那个目录对应的文件夹里即可

4. 重启手机，重新越狱，即可生效

### deb 包
1. 如果你想安装 deb 包插件，则不建议新手操作，因为有的插件存在兼容性问题

2. deb 包需要编译才可以安装，这里以 macOS 为例

3. 为 macOS 安装 Homebrew，在终端执行
    ```
    /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    ```

4. 安装 dpkg
    ```
    brew install dpkg
    ```

5. 从 patcherplus 的项目地址下载 patcherplus 

6. 命令行中 cd 到 patcherplus 所在的目录

7. 修改权限
    ```
    chmod +x ./patcherplus
    ```
    如果你计算机上显示有后缀，上面一句可能不管用，执行
    ```
    chmod +x ./patcherplus.dms
    ```

8. 运行 patcherplus
    ```
    ./patcherplus
    ```
    或者
    ```
    ./patcherplus.dms
    ```

9. 然后编译deb包，选择输出目录即可

10. 之后就按照前面的方式复制进去

## 跳坑
1. 基本思想就是先越狱，建立相关目录，然后拷贝文件，重启再越狱生效

2. 重启都会导致失效，重新越狱即可恢复，越狱后自动注销（重启SpringBoard）才算成功

3. GeoFilza 打开经常会重启，要有耐心

4. 如果你放好文件后，重启越狱老是失败，多半是插件不兼容，删掉即可。卸载也是，删除文件即可。
