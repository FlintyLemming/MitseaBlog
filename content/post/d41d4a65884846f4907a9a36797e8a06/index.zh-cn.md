+++
author = "FlintyLemming"
title = "Windows 笔记本省电待机设置"
slug = "d41d4a65884846f4907a9a36797e8a06"
date = "2024-01-10"
description = "新入手了台笔记本，照例修改了待机策略"
categories = ["Windows", "Consumer"]
tags = ["Windows", "笔记本"]
image = "https://blog-img.mitsea.com/images/blog/posts/2024/01/Windows%20%E7%AC%94%E8%AE%B0%E6%9C%AC%E7%9C%81%E7%94%B5%E5%BE%85%E6%9C%BA%E8%AE%BE%E7%BD%AE/daniil-silantev-eNnGiKM-IjE-unsplash.avif"
+++

## 背景知识

首先我先简要解释 待机、休眠、睡眠 三个状态的含义。请注意，待机 和 睡眠 两个词的理解往往有一定的出入，这里只说我自己的理解。

### 休眠

休眠时，机器会把当前状态保存到硬盘中并关闭电源。休眠后，电脑几乎不耗电，这是除关机外最省电的模式。

但是恢复时由于要从硬盘重新读取数据，所以比较慢。如果你打开盖子后笔记本无反应，需要按一下开机键，然后会有开机转圈的画面，那就是休眠模式。

### 睡眠

在讲睡眠之前，首先先查看自己当前电脑的睡眠状态

1. 按下 Win-R 打开运行
2. 输入 cmd 然后回车
3. 输入 `powercfg /a` 然后回车

然后你就会发现对于 Windows 来说，待机和休眠都是一种睡眠状态

![](https://blog-img.mitsea.com/images/blog/posts/2024/01/Windows%20%E7%AC%94%E8%AE%B0%E6%9C%AC%E7%9C%81%E7%94%B5%E5%BE%85%E6%9C%BA%E8%AE%BE%E7%BD%AE/Untitled.avif)

不过一般来说，休眠指待机（S1）和待机（S3）这两种睡眠状态

睡眠时，所有的程序都会停止，但是信息还保留在内存上。如果笔记本电池用尽，就会丢失未保存的信息。耗电比休眠要多，但是新电脑一般可以在这个状态下睡眠好几天。

睡眠的唤醒速度比较快，一般打开屏幕盖子后，敲一下键盘就能唤醒电脑，直接进入锁屏界面。

由于唤醒速度明显比休眠快，并且耗电也能接受，所以很多人喜欢用这个模式。这也是为什么有不少人想要把 S0 待机 改成 S3 待机，也就是睡眠。

### 待机

待机现在一般指 待机（S0 低电量待机）。这是微软在现代笔记本上为了最大程度追求即开即用的响应速度而推行的一种模式。目前大多数新电脑一般也采用这种模式。此时其实电脑原则上是处在一种工作状态，只是跟现代移动处理器的特性进行了优化，所以理论上耗电非常低。

但是往往会有很多人抱怨待机（S0 低电量待机）非常耗电，甚至电脑合盖后没多久就没电了，这往往是因为该模式默认还保留了活跃的网络链接，这个其实是为了有蜂窝链接的设备以及平板设计的，但是笔记本其实并不需要在盒盖后还要保持网络链接，所以后面会设置如何关闭待机时的网络链接。

### 如何使用

下面我根据场景来推荐你需要使用哪一种方式放置笔记本。如果使用待机，请根据后文的设置来优化待机时的耗电。

如果你的笔记本在未来的数小时内还需要使用，建议使用**待机**；如果平时需要多次开盖合盖，则推荐将盒盖的动作设置为**待机**。

如果你的笔记本较老，没有待机选项，上述的场景应该使用**睡眠**

如果你未来数天都不打算使用笔记本，建议使用**休眠**

## 优化

### 修改休眠为待机

如果你的笔记本合盖的状态为休眠，但是又觉得唤醒太慢，可以修改默认方式为待机或睡眠。

1. 打开控制面板。可以点击开始菜单后，直接打字搜索控制面板来打开。
2. 依次打开 硬件与声音 - 电源选项
3. 打开左侧的 选择关闭笔记本计算机盖的功能
4. 点击更改当前不可用的设置

    ![](https://blog-img.mitsea.com/images/blog/posts/2024/01/Windows%20%E7%AC%94%E8%AE%B0%E6%9C%AC%E7%9C%81%E7%94%B5%E5%BE%85%E6%9C%BA%E8%AE%BE%E7%BD%AE/Untitled%201.avif)

推荐如下设置：

上方都修改为睡眠，关机设置中打开休眠，并考虑关闭睡眠（因为没有必要了）

![](https://blog-img.mitsea.com/images/blog/posts/2024/01/Windows%20%E7%AC%94%E8%AE%B0%E6%9C%AC%E7%9C%81%E7%94%B5%E5%BE%85%E6%9C%BA%E8%AE%BE%E7%BD%AE/Untitled%202.avif)

此时，如果你接下来若干小时还需要使用电脑，那不用电脑时直接合盖即可；如果未来几天都不用，那就点击开始菜单-电源按钮-休眠（或者直接关机，根据情况选择）

如果你不需要使用睡眠，则可以按照下面的方法完全关闭，这会为你剩下等同于内存大小的系统盘空间

1. 右键开始菜单，点击 PowerShell（管理员）或是 终端（管理员）
2. 输入 cmd，回车
3. 输入 `powercfg.exe /hibernate off` ，回车

### 关闭待机时的网络链接

在开头，你已经查看了笔记本的睡眠状态。若当前的睡眠状态是 待机（S3），则不需要修改。如果是 待机（S0 低电量待机）连接的网络，则需要按照下面的步骤进行修改。经过测试，我的笔记本在修改后待机 12 个小时大约消耗 10% 的电量。

![](https://blog-img.mitsea.com/images/blog/posts/2024/01/Windows%20%E7%AC%94%E8%AE%B0%E6%9C%AC%E7%9C%81%E7%94%B5%E5%BE%85%E6%9C%BA%E8%AE%BE%E7%BD%AE/Untitled%203.avif)

1. 搜索并打开注册表编辑器
2. 定位到 `计算机\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\PowerSettings\F15576E8-98B7-4186-B944-EAFA664402D9` ，直接点击这个文件夹，不用展开
3. 双击 Attributes，修改为 2，然后确定，关闭注册表编辑器
4. 打开控制面板，依次打开 硬件与声音 - 电源选项 - 当前选定的计划右侧的 更改计划设置 - 更改高级电源设置
5. 把 待机状态下的网络连接 性下面两个都改为禁用

    ![](https://blog-img.mitsea.com/images/blog/posts/2024/01/Windows%20%E7%AC%94%E8%AE%B0%E6%9C%AC%E7%9C%81%E7%94%B5%E5%BE%85%E6%9C%BA%E8%AE%BE%E7%BD%AE/Untitled%204.avif)

6. 再次查看此系统上的睡眠状态，变成 待机（S0 低电量待机）网络已断开连接 即可

    ![](https://blog-img.mitsea.com/images/blog/posts/2024/01/Windows%20%E7%AC%94%E8%AE%B0%E6%9C%AC%E7%9C%81%E7%94%B5%E5%BE%85%E6%9C%BA%E8%AE%BE%E7%BD%AE/Untitled%205.avif)

> Photo by [Daniil Silantev](https://unsplash.com/@betagamma?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/a-group-of-trees-that-are-covered-in-snow-eNnGiKM-IjE?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)
