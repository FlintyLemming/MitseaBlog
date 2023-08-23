+++
author = "FlintyLemming"
title = "无痛禁用 Microsoft Edge"
slug = "da24ec5e61424af8a19442946c225c1a"
date = "2023-08-23"
description = "Windows 自带流氓软件"
categories = ["LifeTec", "Microsoft", "Windows"]
tags = ["Edge"]
image = "https://img.mitsea.com/blog/posts/2023/08/%E6%97%A0%E7%97%9B%E7%A6%81%E7%94%A8%20Microsoft%20Edge/philip-oroni-KtEloTGQ8wM-unsplash.jpg?x-oss-process=style/ImageCompress"
+++

如果通过 Powershell 命令强制卸载 Edge 软件包，可能会导致意料之外的问题，推荐使用策略组编辑器禁止 Edge 运行

如果系统是 Windows 10/11 专业版，系统中有策略组编辑器，可以按照下面方法禁用

1. 按下 Win+R 键，在运行输入框内输入 gpedit.msc，点击确定打开组策略
2. 左边找到 用户配置 - 管理模板 - 系统。点击 系统 即可，不要展开它。
3. 双击 不运行指定的 Windows 应用程序
4. 在弹出窗口中，将左上角的配置状态改为 已配置
5. 点击 不允许的应用程序列表 右侧的 显示…
6. 添加一项 `msedge.exe` 后，点击 确定

    ![](https://img.mitsea.com/blog/posts/2023/08/%E6%97%A0%E7%97%9B%E7%A6%81%E7%94%A8%20Microsoft%20Edge/Untitled.png?x-oss-process=style/ImageCompress)

7. 再点击 确定，然后关闭 策略组编辑器 即可

如果是 Windows 10/11 家庭版用户，可以先用下面的方法启用策略组编辑器（方法来自互联网）。然后再按照上面的方法打开策略组编辑器操作

1. 在桌面上创建一个记事本，然后把代码粘贴进去；

    代码如下（示例）：

    ```cmd
    @echo offpushd "%~dp0"dir /b C:\Windows\servicing\Packages\Microsoft-Windows-GroupPolicy-ClientExtensions-Package~3*.mum >List.txtdir /b C:\Windows\servicing\Packages\Microsoft-Windows-GroupPolicy-ClientTools-Package~3*.mum >>List.txtfor /f %%i in ('findstr /i . List.txt 2^>nul') do dism /online /norestart /add-package:"C:\Windows\servicing\Packages\%%i"pause
    ```

2. 保存之后将拓展名改为 .cmd
3. 右击 cmd 文件，选择以管理员身份运行，运行后记得重启电脑

> Photo by [Philip Oroni](https://unsplash.com/@philipsfuture?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/photos/a-computer-generated-image-of-a-blue-and-black-object-KtEloTGQ8wM?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
  