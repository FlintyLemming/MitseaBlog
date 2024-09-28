+++
author = "FlintyLemming"
title = "MacBook Pro 2018 运行 Windows to Go 的一些坑"
slug = "cad4783b22274fcca2306d9bdf5355b9"
date = "2020-02-06"
description = ""
categories = ["Apple", "Windows"]
tags = ["MacBook", "wtg"]
image = "https://hf-public-source.mitsea.com:8840/images/blog/posts/2020/02/MacBook%20Pro%202018%20%E8%BF%90%E8%A1%8C%20Windows%20to%20Go%20%E7%9A%84%E4%B8%80%E4%BA%9B%E5%9D%91/steve-johnson-KvW2dYlAkfM-unsplash.avif"
+++

前几个月买了个 MacBook Pro 2018 13‘，配置是 i5-8259U - 16G - 256G - Iris Plus 655。由于这是我唯一的个人电脑，所以除了自带的 macOS ，我会使用 Windows，主要是使用外置显卡进行学习和娱乐。下面我就来细说一下自己遇到的一些坑，当然，下面提到的很多问题我都没有确定具体产生原因，只是给出了我自己的有限的解决方案。需要说明的是，以下提到的情况可能是我自己这台电脑独有的问题，也许不具备参考性，请自行斟酌。

## e-GPU

### 信息介绍

我在淘宝购买的外置显卡扩展坞，不知道怎么分享链接比较好，我就说一下店家吧，叫 **金捷海量**。我购买的是最便宜的型号，不支持3风扇卡，也不支持使用时给笔记本充电。看了一下芯片信息和结构，用的是 LT-LINK 的雷电3板子然后扩展出 PCI-E，有一个小电源。

### 槽点

对于显卡坞本身没什么好吐槽的，毕竟价格摆在这，就是内置的风扇噪音有点大，回头打算换了。
下面说说系统兼容性，对于 2018 款 MacBook Pro，macOS 下毫无悬念，只能用新的A卡，RX 4xx、RX 5xx之类的，老的 GT640 这种免驱卡我就不知道能不能用了，没准都不支持外接。这里有几点要注意：RX 580 2048sp 是不可以用的（不知道刷 BIOS 管不管用）；RX 590 驱动正常，但是认不出来卡名字，会显示成是一张 Prototype 卡；其他卡插上去有可能认错卡名字，比如你插了一张 580，有可能给你识别成 480；不建议用 xfx 的卡，有的要刷 BIOS 才行，具体自己查一下。Windows 就复杂了，Nvidia 的卡倒是基本都能用，没啥限制，我用的是 RTX 2060。但是 A 卡就麻烦了，要诱骗什么的，具体我也没研究，总之想要用起来很麻烦，毫无体验。所以总的来说，你要既想在 macOS 下用 eGPU，又想在 Windows 下用，那你最好就是买两个扩展坞，一个插A卡，一个插N卡……
再说说使用问题，如果你用 Windows to Go + 外置显卡，那么你的外置硬盘必须插在左边，而且最好是靠后的那个口，靠前的那个有可能卡在 Windows 启动界面进不去系统；eGPU 雷电3的线必须插在右边，前后我目前观察无所谓。正常进入系统后用着倒是没啥问题，macOS 和 Windows 都还算正常，特别是 Windows，就完全当成一个台式机用，也不用管软件会不会掉用独立显卡，这些都是自动的，而且资源分配完全没有问题。这里提一下 macOS，在 macOS 下，不要想着用 eGPU 玩游戏了…支持很差。除了少数比如 神界原罪 这种在设置里可以选择渲染的 GPU，大部分游戏都不支持选择，然后的结果就是比核芯显卡表现好一些，但跟用独立显卡差太多，就算在应用的简介里勾选优先使用 eGPU 都没用。

## 外置硬盘

### 信息介绍

除了装有 Windows 的 wtg 移动硬盘，我还额外使用了一个移动硬盘扩展存储。是 建兴T11 PLUS 512G nvme 固态硬盘+金胜 nvme 硬盘盒，看了一下硬盘盒的主控，是镁光的。

### 槽点

这个硬盘，在没有数据流一段时间后，就会与系统断开连接，再用的话就只能先拔掉，再插上。我检查了关于硬盘休眠的设置，没有什么问题。最后想出来一个办法，就是把 QQ 和 QQ文档 路径都放在这个盘里，保持一直有数据交换，用了一段时间，没有断开的问题了。

## 外设扩展使用

一个C口接 wtg 移动硬盘、一个C口接显卡，那么使用 Windows to Go 就不可避免得使用扩展坞了。先说下我的扩展坞，是绿联的 Type-C 扩展坞，功能最多的那一款，VGA、RJ45啥的都有的那个。

### 扩展坞本身

这个扩展坞的的 USB-A 口设计的就有点问题，口小了一圈，导致我有的U盘甚至都插不进去，要很用力很用力。还有就是他支持外接供电然后给笔记本充电，但是有时充不了，可能是接触不良。
最重要的问题就是，如果你在进入 Windows 前不插好你需要使用的设备，那么进入 Windows 后，可能就无法使用。比如有时你需要用数据线连接你的 iPhone，抱歉不行，你得插上后，重启，然后才有效。

### 外置硬盘

没错，在扩展坞上，我又接了一个外置硬盘盒，内有两块机械硬盘。问题就在于，如果你在开机选择引导的时候不把硬盘盒接到扩展坞上，那么开机后或者选择引导后再插入硬盘盒就有可能系统认不到硬盘，甚至无法启动系统。但如果你在插着硬盘盒的情况下进开机引导就会非常慢，甚至进不了引导，就是按着 Option 但是一直卡在苹果 logo 处，因为除了 wtg 那个硬盘，电脑还要检索其他磁盘。

### 外部输入设备

这是最近出的问题，可能是在我 Windows 下配对过 MagicKeyboard 后出现的问题。就是当我蓝牙或者有线接 Xbox One 手柄后一段时间，键盘无法正确响应操作，具体表现为好像按住了某个按键，比如你双击“计算机”，打开的却是属性。

## 总结

我最早接触的 MacBook 是 2006 年款的，那种全白色塑料的。当时我也有用 Windows，印象中没什么问题，就像一台普通的 PC。但是这么多年过去了，当我再次使用 MacBook 时，Windows 下的表现没有想象中的那么好了。倒不是说 MacBook Pro 本身有什么问题，毕竟可能因为它有 T2 芯片，遇到各种各样的问题无可厚非，再说苹果貌似也不为 Windows 下出现的问题负责。但对我这种只有一台电脑的人来说却是有点不太友好，我的建议还是如果需要用 Windows，还是再弄一台 PC 会比较好。不过尽管我吐槽了这么多地方，如果你的使用场景比较固定，我觉得 Windows 用起来问题也不是太大，说白了还是取决于你的钱包能不能再搞一台 PC 来用了(