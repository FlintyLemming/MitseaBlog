+++
author = "FlintyLemming"
title = "国行版小米盒子3s刷国际版系统"
slug = "b053f84f0b174a088c57d4ec93b4da64"
date = "2020-02-09"
description = ""
categories = ["Android", "Consumer"]
tags = ["小米"]
image = "https://image.mitsea.com/blog/posts/2020/02/%E5%9B%BD%E8%A1%8C%E7%89%88%E5%B0%8F%E7%B1%B3%E7%9B%92%E5%AD%903s%E5%88%B7%E5%9B%BD%E9%99%85%E7%89%88%E7%B3%BB%E7%BB%9F/title.avif"
+++

*主要内容来自[http://www.hdpfans.com/thread-777808-1-1.html](http://www.hdpfans.com/thread-777808-1-1.html)，其他相关连接：*

[http://miui.vn/forum/threads/cai-dat-firmware-global-cho-mi-box-3s-mdz-19-aa.50835/](http://miui.vn/forum/threads/cai-dat-firmware-global-cho-mi-box-3s-mdz-19-aa.50835/)

## 需要注意的是

1. 本人第一次研究 Android TV，所以操作上可能存在问题
2. 扒的教程，可能并不是最简单的步骤，但是可行性较高

## 准备工具

1. 一个能科学上网的路由器，或者人在海外
2. 小米盒子3s
3. U盘（建议不大于32G，不然可能会出现问题）
4. USB双头线（可以不需要，教程也是以不用双头线为例）

## 具体步骤

### 降级

1. 格式化U盘为FAT32，如果是NTFS，在第4步可能无法正常刷机，原来是FAT32的话可以直接使用
2. 解压**MiBOX3S_queenchristina_r145.rar**里的两个文件，并将文件放入U盘根目录下
3. 将U盘插入盒子，断电，然后同时按住遥控器上Home和Menu键，此时通电开机
4. 盒子会自动进入刷机界面，如果未自动刷机，出现的是recovery界面，则要检查U盘分区格式是否为FAT32
5. 等待刷机成功后进入系统

### 再刷第二个固件（个人猜测是包含recovery的一个固件，不是特别懂）

1. 盒子上自行安装文件管理器，将**dump_16AB.img**通过U盘拷贝到盒子文件系统的**sdcard**目录下，拷贝完成后拔出U盘

    > 原教程是在建立adb连接后拷贝文件，个人用adb push命令尝试拷贝文件无果，所以建议直接用U盘复制

2. 进入小米盒子系统的设置-账户与安全，在里面打开USB调试
3. 在Wi-Fi设置里获取盒子的局域网IP地址
4. 电脑上进入命令行，使用adb网络调试连接盒子，输入

    ```bash
    adb connect <盒子的ip地址>
    ```

    系统会自动补上端口号进行连接（默认是5555）

5. 接着依次输入如下命令

    ```bash
    adb root
    adb remount
    adb shell dd if=/sdcard/dump_16AB.img of=/dev/block/mmcblk0
    ```

    > 原教程指出第三条命令会花费相当长时间，需要耐心等待跳出新索引箭头，个人可能是因为使用网络adb原因，始终没有跳出新索引箭头，在等待一小时左右后直接关闭窗口，强制执行下面一步，也没有问题

6. 断电，然后同时按住遥控器上Home和Menu键，此时通电开机，进入recovery界面，进行双清
7. 将**MiBOX3_user_once_r750**文件夹里的文件放入U盘根目录
8. 插入U盘，选择Choose Apply update from EXT > Update from udisk，然后选择拷贝的文件，刷入国际版系统
9. 等待重启就可以进入Android TV系统了，完成相关设置即可

    > 进入系统后首先要检查你是否可以正常安装第三方app，我个人遇到的情况就是无法安装绝大多数第三方app，原教程中没有提及这个问题，如果存在这个问题，请继续下一步

10. 解压**MiBOX3_userdebug_once_r454.rar**里的两个文件，并将文件放入U盘根目录下（原来的那个记得删除）
11. 将U盘插入盒子，断电，然后同时按住遥控器上Home和Menu键，此时通电开机
12. 等待刷机，成功后则进入系统，重新设置后发现新的系统可以正常安装app
