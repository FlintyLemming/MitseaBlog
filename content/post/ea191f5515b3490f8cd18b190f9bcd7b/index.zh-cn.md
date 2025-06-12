+++
author = "FlintyLemming"
title = "【归档】iOS 刺激战场 解锁极限帧率"
slug = "ea191f5515b3490f8cd18b190f9bcd7b"
date = "2020-05-27"
description = ""
categories = ["Game"]
tags = ["FPS"]
image = "https://img.flinty.moe/blog/posts/2020/05/iOS%20%E5%88%BA%E6%BF%80%E6%88%98%E5%9C%BA%20%E8%A7%A3%E9%94%81%E6%9E%81%E9%99%90%E5%B8%A7%E7%8E%87/title.avif"
+++

> 游戏已改名为 和平精英，方法仍然有效
> 

## 警告⚠️，你正在进行危险操作

**你正在通过修改游戏文件获得游戏内优势，这本质上可以被封禁账号。你正在通过修改预置的画质修改对设备性能的调用，这可能会导致你的设备受到不可逆的损伤，并可能无法被保修修改后游戏可能会让设备过热触发过热锁定，请自备湿纸巾等有效降温手段**

![](https://img.flinty.moe/blog/posts/2020/05/iOS%20%E5%88%BA%E6%BF%80%E6%88%98%E5%9C%BA%20%E8%A7%A3%E9%94%81%E6%9E%81%E9%99%90%E5%B8%A7%E7%8E%87/1.avif)

![](https://img.flinty.moe/blog/posts/2020/05/iOS%20%E5%88%BA%E6%BF%80%E6%88%98%E5%9C%BA%20%E8%A7%A3%E9%94%81%E6%9E%81%E9%99%90%E5%B8%A7%E7%8E%87/2.avif)

## 简要介绍

iOS 的刺激战场往往是不能全开特效的，尤其是开不了 HDR高清+极限。而我的 iPhone 6s 连 流畅+极限 都开不了，显然以 A9 的性能和 6s 的分辨率，流畅+极限还是ok的，这里我就尝试开启。

当然这个方法 iPad 也能用，我的 iPad Pro 10.5 实测也是可以用的（中间也穿插了几张使用 iPad 操作的截图）

![](https://img.flinty.moe/blog/posts/2020/05/iOS%20%E5%88%BA%E6%BF%80%E6%88%98%E5%9C%BA%20%E8%A7%A3%E9%94%81%E6%9E%81%E9%99%90%E5%B8%A7%E7%8E%87/3.avif)

## 需要的工具

1. 能运行刺激战场的 iOS 设备
2. 一台装有 Windows 或 macOS 的电脑
3. 软件 iMazing

## 下载 iTunes 【如已安装可跳过】

这里需要注意，iMazing无法识别通过Windows商店安装的iTunes所附带的驱动（尽管爱思助手都已经支持）
所以你必须去苹果官网下载x86架构的iTunes，网址在这里：
[https://support.apple.com/zh-cn/HT208079](https://support.apple.com/zh-cn/HT208079)
一般选择64位即可

## 下载iMazing

iMazing推荐去官网下载，由于本次操作并不需要高级版功能，所以直接下载使用就行，无需购买
官网链接：[https://imazing.com/zh](https://imazing.com/zh)

## 备份刺激战场应用数据

这是最难受的，因为如果你是第一次提取应用存档数据，那么他是需要备份你设备的所有数据，所以如果你是256G或者512G设备并且文件都塞很多，那就要备份很久

1. 如果你不打算备份到C盘，首先你要修改默认备份地址，这一步强烈建议操作。打开偏好设置
    
    ![](https://img.flinty.moe/blog/posts/2020/05/iOS%20%E5%88%BA%E6%BF%80%E6%88%98%E5%9C%BA%20%E8%A7%A3%E9%94%81%E6%9E%81%E9%99%90%E5%B8%A7%E7%8E%87/4.avif)
    
2. 在**备份**选项卡里修改默认备份位置，点击这个下拉框，然后点击**选择**选择一个自己想要保存备份的位置
    
    ![](https://img.flinty.moe/blog/posts/2020/05/iOS%20%E5%88%BA%E6%BF%80%E6%88%98%E5%9C%BA%20%E8%A7%A3%E9%94%81%E6%9E%81%E9%99%90%E5%B8%A7%E7%8E%87/5.avif)
    
3. 点备份，我们要备份设备的全部数据，因为这个软件要提取某个应用的数据文件，就需要备份整个设备
    
    ![](https://img.flinty.moe/blog/posts/2020/05/iOS%20%E5%88%BA%E6%BF%80%E6%88%98%E5%9C%BA%20%E8%A7%A3%E9%94%81%E6%9E%81%E9%99%90%E5%B8%A7%E7%8E%87/6.avif)
    
4. 这里注意，一定要确认选择的是刚刚新创建的备份路径
    
    ![](https://img.flinty.moe/blog/posts/2020/05/iOS%20%E5%88%BA%E6%BF%80%E6%88%98%E5%9C%BA%20%E8%A7%A3%E9%94%81%E6%9E%81%E9%99%90%E5%B8%A7%E7%8E%87/7.avif)
    
    由于第一次需要全部备份，所以时间非常久。我平板有将近200G数据，备份要将一个多小时。
    
    ![](https://img.flinty.moe/blog/posts/2020/05/iOS%20%E5%88%BA%E6%BF%80%E6%88%98%E5%9C%BA%20%E8%A7%A3%E9%94%81%E6%9E%81%E9%99%90%E5%B8%A7%E7%8E%87/8.avif)
    
5. 备份好后，关闭窗口回到主页面，点“管理应用程序”
    
    ![](https://img.flinty.moe/blog/posts/2020/05/iOS%20%E5%88%BA%E6%BF%80%E6%88%98%E5%9C%BA%20%E8%A7%A3%E9%94%81%E6%9E%81%E9%99%90%E5%B8%A7%E7%8E%87/9.avif)
    
6. 点击**设备**选项卡，不要在资料库里找
    
    ![](https://img.flinty.moe/blog/posts/2020/05/iOS%20%E5%88%BA%E6%BF%80%E6%88%98%E5%9C%BA%20%E8%A7%A3%E9%94%81%E6%9E%81%E9%99%90%E5%B8%A7%E7%8E%87/10.avif)
    
7. 找到刺激战场，然后右键，点“备份应用程序数据”
    
    ![](https://img.flinty.moe/blog/posts/2020/05/iOS%20%E5%88%BA%E6%BF%80%E6%88%98%E5%9C%BA%20%E8%A7%A3%E9%94%81%E6%9E%81%E9%99%90%E5%B8%A7%E7%8E%87/11.avif)
    
8. 这里注意选择“从最近备份提取”，存档文件默认就保存在桌面
    
    ![](https://img.flinty.moe/blog/posts/2020/05/iOS%20%E5%88%BA%E6%BF%80%E6%88%98%E5%9C%BA%20%E8%A7%A3%E9%94%81%E6%9E%81%E9%99%90%E5%B8%A7%E7%8E%87/12.avif)
    

## 修改画质

1. 然后就能在桌面上找到刚才的文件了，文件名为“绝地求生：刺激战场.imazingapp”，右键，选择用好压打开。
    
    > 这里注意，我后面的操作以好压为例，因为好压可以直接编辑压缩包内的文件内容，如果你用其他的不能编辑的话，解压后编辑再压缩回去我试过一次不管用，我也不知道为什么，所以我建议用好压处理这个文件。用完你可以卸载。
    > 
2. 定位到 \Container\Documents\ShadowTrackerExtra\Saved\Config\IOS，找到文件 UseCustom.ini
3. 右键这个文件，使用内部查看器打开
4. 在 \[UserCustom DeviceProfile] （注意不是\[BackUp DeviceProfile]）下面添加四行：
+CVars=r.PUBGDeviceFPSLow=60
+CVars=r.PUBGDeviceFPSMid=60
+CVars=r.PUBGDeviceFPSHigh=60
+CVars=r.PUBGDeviceFPSHDR=60
然后按一下回车（玄学操作，看不懂）
最后效果如图
    
    ![](https://img.flinty.moe/blog/posts/2020/05/iOS%20%E5%88%BA%E6%BF%80%E6%88%98%E5%9C%BA%20%E8%A7%A3%E9%94%81%E6%9E%81%E9%99%90%E5%B8%A7%E7%8E%87/13.avif)
    
    保存关闭窗口
    
5. 这里确认修改
    
    ![](https://img.flinty.moe/blog/posts/2020/05/iOS%20%E5%88%BA%E6%BF%80%E6%88%98%E5%9C%BA%20%E8%A7%A3%E9%94%81%E6%9E%81%E9%99%90%E5%B8%A7%E7%8E%87/14.avif)
    
6. 回到 iMazing，找到刺激战场，恢复存档。存档就选择刚才那个文件
    
    ![](https://img.flinty.moe/blog/posts/2020/05/iOS%20%E5%88%BA%E6%BF%80%E6%88%98%E5%9C%BA%20%E8%A7%A3%E9%94%81%E6%9E%81%E9%99%90%E5%B8%A7%E7%8E%87/15.avif)
    
7. 恢复过程中可能会要求你关闭查找设备，这里必须关闭，一会弄好了再打开就行。然后设备会软重启一次，等待设备提示输入密码，并进入系统，打开游戏。这个时候你就会发现已经可以打开所有画质选项了
8. 选择好你要的画质后，点击确认修改。这个时候理论上可以用了，但是为了防止下次打开后失效还需要进行一些操作。
9. 修改好画质后，手机在游戏里不要动，再提取一次刺激战场的存档，注意这里选择“备份并提取应用程序数据”，这一步也要等一会。注意，等待过程中千万不要操作手机！
    
    ![](https://img.flinty.moe/blog/posts/2020/05/iOS%20%E5%88%BA%E6%BF%80%E6%88%98%E5%9C%BA%20%E8%A7%A3%E9%94%81%E6%9E%81%E9%99%90%E5%B8%A7%E7%8E%87/16.avif)
    
10. 然后再找到刚才那个文件，发现变回原样了，再编辑一次，然后保存文件。再恢复一次存档
11. 重启手机进入游戏，就发现刚才设置的画质还在。至此，画质修改完毕。