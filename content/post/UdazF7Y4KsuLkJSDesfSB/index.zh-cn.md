+++
author = "FlintyLemming"
title = "QNAP TS-453b mini 恢复引导 救砖"
slug = "UdazF7Y4KsuLkJSDesfSB"
date = "2025-05-26"
description = "其实就是根目录空间不够系统升级"
categories = ["HomeLab"]
tags = ["NAS"]
image = "https://img.flinty.moe/blog/posts/2025/05/QNAP%20TS-453b%20mini%20%E6%81%A2%E5%A4%8D%E5%BC%95%E5%AF%BC%20%E6%95%91%E7%A0%96/squids-z-D5HE4cj-oIQ-unsplash.avif"
+++

## 准备事项

1. 准备一个U盘
2. 机器如果插了两根 8G 的内存，建议拔掉一个
3. 由于机器开机即给硬盘上电，恢复过程中需要多次重启，所以全程建议不要插硬盘

## 恢复引导盘

恢复过程比较简单，参考下面的官方链接恢复即可，本质就是把他提供的一个 img 文件 dd 到内置的引导盘上

[x86架构NAS恢复指南 ](https://www.qnap.com.cn/zh-cn/how-to/faq/article/nas-recovery-guide-for-x86-based-nas)

[NAS 恢复指南，适用于 TS-453Bmini、TS-269 Pro、TS-269L、TS-x79、TS-x70、TS-x80、TS-ECx80U、SS-ECx79U](https://www.qnap.com.cn/zh-cn/how-to/faq/article/firmware-recovery-guide-for-ts-453bmini-ts-269-pro-ts-269l-ts-x79-ts-x70-ts-x80-ts-ecx80u-ss-ecx79u-series-nas)

如果不熟悉 Linux 的话，其实自己做一个 PE 启动盘，然后把 Rufus 便携版的 exe 和官网提供的 img 复制到 U 盘的数据分区里

进到 PE 后，打开 Rufus，磁盘就选择那个 515MB 的小硬盘，选择下载好的 img 恢复即可

![](https://img.flinty.moe/blog/posts/2025/05/QNAP%20TS-453b%20mini%20%E6%81%A2%E5%A4%8D%E5%BC%95%E5%AF%BC%20%E6%95%91%E7%A0%96/TT%E6%88%AA%E5%9B%BE%E6%9C%AA%E5%91%BD%E5%90%8D_F1mqdyO42P.avif)

## 升级系统（问题分析）

这一节只是分析问题，适合遇到坑之后才找到这篇文章的人看。正常恢复的可以直接跳到下一节。

恢复好引导后，就能正常开机了，然后在 Qfinder Pro 里会找到设备，按照官方教程来说，你应该在这里右键更新系统软件

但是你用官方现在提供的任何一个版本更新都会失败

![](https://img.flinty.moe/blog/posts/2025/05/QNAP%20TS-453b%20mini%20%E6%81%A2%E5%A4%8D%E5%BC%95%E5%AF%BC%20%E6%95%91%E7%A0%96/%E5%9B%BE%E7%89%87_w62aj9TV64.avif)

此时设备名称也会变成 uLinux，我建议是重启一下，他会恢复，不然后续如果你想再用 Qfinder Pro 操作升级会无法上传固件。为了探究失败原因，使用 ssh 进入到设备控制台看一下，执行 `df -h` 后发现 `/` 已经 100% 了，这可能就是导致升级失败的原因

![](https://img.flinty.moe/blog/posts/2025/05/QNAP%20TS-453b%20mini%20%E6%81%A2%E5%A4%8D%E5%BC%95%E5%AF%BC%20%E6%95%91%E7%A0%96/%E5%9B%BE%E7%89%87_BGOebPEKLW.avif)

可以进一步验证一下，比如可以把升级固件上传到 `/mnt/.fw_update_dir` 里，然后执行下面的命令尝试更新系统

```bash 
 /etc/init.d/update.sh /mnt/.fw_update_dir/{FIRMWARE_FILE_NAME}.img
```


发现它确实就是报空间不足的错误

![](https://img.flinty.moe/blog/posts/2025/05/QNAP%20TS-453b%20mini%20%E6%81%A2%E5%A4%8D%E5%BC%95%E5%AF%BC%20%E6%95%91%E7%A0%96/%E5%9B%BE%E7%89%87_63R9ouwoud.avif)

此时我们可以删除掉一些不影响升级的文件腾出空间，比如 `home`下面的文件夹

```bash 
rm -rf /home/httpd
rm -rf /home/Qhttpd
```


此时再重新使用命令行或者 Qfinder Pro 升级就可以正常升级

## 系统升级（详细步骤）

1. 从官网下好最新版的系统固件，并解压出 img 文件
2. 恢复后，打开 Qfinder Pro，确认能看到一个系统版本只有 1.3.0 的设备，并记下他的 IP

   ![](https://img.flinty.moe/blog/posts/2025/05/QNAP%20TS-453b%20mini%20%E6%81%A2%E5%A4%8D%E5%BC%95%E5%AF%BC%20%E6%95%91%E7%A0%96/%E5%9B%BE%E7%89%87_6NRv18y8qG.avif)
3. 打开任意 SSH 工具，可以直接使用 Powershell，连接到 NAS 上
   ```bash 
   ssh admin@{IP}
   ```

   密码也是 admin
4. 由于默认根目录过小会导致升级失败，所以我们需要删除一些不影响升级的文件，比如 home 下的两个文件夹
   ```bash 
   rm -rf /home/httpd
   rm -rf /home/Qhttpd
   ```

5. 回到 Qfinder Pro，右键设备，点击 更新系统软件

   ![](https://img.flinty.moe/blog/posts/2025/05/QNAP%20TS-453b%20mini%20%E6%81%A2%E5%A4%8D%E5%BC%95%E5%AF%BC%20%E6%95%91%E7%A0%96/%E5%9B%BE%E7%89%87_yKUDnPnGRw.avif)

   账号密码都是 admin

   ![](https://img.flinty.moe/blog/posts/2025/05/QNAP%20TS-453b%20mini%20%E6%81%A2%E5%A4%8D%E5%BC%95%E5%AF%BC%20%E6%95%91%E7%A0%96/%E5%9B%BE%E7%89%87_odH1l_K1xX.avif)

   稍等一会，它会提示你版本已经是最新，不用管他，直接确定

   ![](https://img.flinty.moe/blog/posts/2025/05/QNAP%20TS-453b%20mini%20%E6%81%A2%E5%A4%8D%E5%BC%95%E5%AF%BC%20%E6%95%91%E7%A0%96/%E5%9B%BE%E7%89%87_DNk5DFAdv1.avif)
6. 选择下载并解压好的最新版 5.x 固件，然后点击 开始更新

   ![](https://img.flinty.moe/blog/posts/2025/05/QNAP%20TS-453b%20mini%20%E6%81%A2%E5%A4%8D%E5%BC%95%E5%AF%BC%20%E6%95%91%E7%A0%96/%E5%9B%BE%E7%89%87_wysopC-dWu.avif)

   它会提示一个这个，不用管他，点击 是

   ![](https://img.flinty.moe/blog/posts/2025/05/QNAP%20TS-453b%20mini%20%E6%81%A2%E5%A4%8D%E5%BC%95%E5%AF%BC%20%E6%95%91%E7%A0%96/%E5%9B%BE%E7%89%87_mfi9zrqfuv.avif)

   能超过 23% 就不会有问题了，等他正常走完，机器会自动重启

   ![](https://img.flinty.moe/blog/posts/2025/05/QNAP%20TS-453b%20mini%20%E6%81%A2%E5%A4%8D%E5%BC%95%E5%AF%BC%20%E6%95%91%E7%A0%96/%E5%9B%BE%E7%89%87_Q1YtEzYFYw.avif)
7. 机器重启后，就可以正常看到运行最新版系统的设备了，重启挺慢的，多等一会。

   ![](https://img.flinty.moe/blog/posts/2025/05/QNAP%20TS-453b%20mini%20%E6%81%A2%E5%A4%8D%E5%BC%95%E5%AF%BC%20%E6%95%91%E7%A0%96/%E5%9B%BE%E7%89%87_GBpv617BFT.avif)

   然后如果显示器已经显示设备进系统了，这个窗口就可以关闭了，重开一下 Qfinder 就能正常看到设备了

   ![](https://img.flinty.moe/blog/posts/2025/05/QNAP%20TS-453b%20mini%20%E6%81%A2%E5%A4%8D%E5%BC%95%E5%AF%BC%20%E6%95%91%E7%A0%96/%E5%9B%BE%E7%89%87_MF_xBh5PX3.avif)

> Photo by [Squids Z](https://unsplash.com/@squids93?utm_content=creditCopyText\&utm_medium=referral\&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/handrails-with-warm-lighting-provide-a-unique-perspective-D5HE4cj-oIQ?utm_content=creditCopyText\&utm_medium=referral\&utm_source=unsplash)
