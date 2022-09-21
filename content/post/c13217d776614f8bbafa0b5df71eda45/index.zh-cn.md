+++
author = "FlintyLemming"
title = "【归档】黑苹果安装与升级"
date = "2020-05-30"
description = ""
categories = ["Apple"]
tags = ["黑苹果"]
image = "https://img.mitsea.com/blog/posts/2020/05/%E9%BB%91%E8%8B%B9%E6%9E%9C%E5%AE%89%E8%A3%85%E4%B8%8E%E5%8D%87%E7%BA%A7/title.jpg?x-oss-process=style/ImageCompress"
+++

> 本文不是手把手教程，仅仅是梳理出一个总的过程，方便在趴教程时心里有个框架
> 

黑苹果的安装主要分为以下几个部分：下载、制作安装盘、配置clover文件、对目标磁盘进行分区、安装、额外驱动安装

1. 下载
macOS 系统为了方便，建议下载已经打包 Clover 的安装文件，文件格式为 .dmg
类似这样的都是已经内置 Clover 的，他们在标题中标注带有 Clover
    
    ![](https://img.mitsea.com/blog/posts/2020/05/%E9%BB%91%E8%8B%B9%E6%9E%9C%E5%AE%89%E8%A3%85%E4%B8%8E%E5%8D%87%E7%BA%A7/1.png?x-oss-process=style/ImageCompress)
    
    什么是 Clover，Clover 是一个引导工具，我们开机自检完毕后首先进入的是引导，如果你是单系统，往往看不到这个界面。Windows 自带的引导显然不能引导 macOS 启动，所以我们需要 Clover 来引导 macOS 启动，当然，它也可以引导 Windows 和 Linux ，是一个很方便的多系统引导工具。
    
2. 制作安装盘
如果你有 Windows 安装经验，这一步就很好理解了，就是简单的刻盘。
如果你使用 macOS ，可以直接制作安装盘，只需要将 EFI 文件夹放入刻录好的安装盘的引导分区即可
这里主要以 Windows 下制作安装盘为例，首先需要一个软件：Transmac，可以方便地刻录 macOS 的安装盘。
以管理员身份打开 Transmac，不然是无法对可移动磁盘进行操作的
插入准备好的 U盘 或者 移动硬盘，左侧找到对应盘符，右键，格式化
然后 restore 下载的 .dmg 文件，等待一会，安装盘就基本制作好了
3. 配置 Clover 文件
安装盘制作好后，会有两个分区，一个是 ESP 分区，用来引导，也是 Clover 所在分区，另一个是 macOS 分区，是系统安装文件。有的情况下 ESP 分区会直接显示出来，名字一般为 EFI 。如果不能显示，可以使用 DiskGenius 编辑里面的文件。
一般我们下载的自带 Clover 的安装包中，会有很多预设的 config.plist 文件，我们可以先选择一个接近当前电脑配置的文件，改名为 config.plist。
对目标磁盘进行分区
macOS 所在磁盘建议为此分配两个分区，这里我们可以用 DiskGenius 自带的一键分区进行分区。
4. 安装
安装其实是最容易出现问题的过程，这大多数都是因为 config.plist 文件配置不当造成的，出现问题不用慌张，每出现一个问题，都记录下并去百度查询，一般都是增删 EFI 分区中的文件，或者修改 config.plist 中的参数，一定要有耐心。
下面简述安装流程：
重启电脑，用制作好的可移动磁盘（UEFI选项）引导启动。
进入 Clover 界面，选择唯一一个 “Install” 选项（名字可能比较长，为了后面的描述，我们称之为 Install 1 选项）
等待滚动log，进入安装的第一阶段，会看到一个 UI 界面
首先进入 磁盘工具，进入后，找到刚才分区的磁盘。虽然我们分出了一个 ESP引导分区，一个文件分区，但其实这里我们只能看到一个 NTFS 文件分区。类似这样：
    
    ![](https://img.mitsea.com/blog/posts/2020/05/%E9%BB%91%E8%8B%B9%E6%9E%9C%E5%AE%89%E8%A3%85%E4%B8%8E%E5%8D%87%E7%BA%A7/2.png?x-oss-process=style/ImageCompress)
    
    选中这个分区，点 抹掉，名称随便起，格式选择 Mac OS 扩展（日志式），然后点 抹掉。这个过程中可能会等很久，甚至会重启，没关系，操作第二次是很正常的。
    
    ![](https://img.mitsea.com/blog/posts/2020/05/%E9%BB%91%E8%8B%B9%E6%9E%9C%E5%AE%89%E8%A3%85%E4%B8%8E%E5%8D%87%E7%BA%A7/3.png?x-oss-process=style/ImageCompress)
    
    然后关闭磁盘工具，选择安装 macOS，安装向导中选择刚刚分好的区，进行安装就可以了，安装过程可能会重启一到两次，重启后进入 Clover 后注意选择项目，不是选择上面的 Install 1 选项，而是选择另外一个带有 Install 的选项（称之为 Install 2）。注意，之后如果安装过程中重启，全部选择 Install 2，包括之后的系统更新。
    安装好后就能进入系统了。检查一下显卡、声卡、网卡驱动是否正常，如果不正常，可以百度一下相关驱动，安装一般都很简单。
    
5. 对于N卡、特殊的声卡型号等等都需要安装后另外安装驱动。
6. 升级
后续升级也很简单，小版本一般直接升级没什么大问题，偶尔会调驱动，如果掉驱动，显卡驱动掉的最多，重打一遍问题不大。只需要注意在系统里下载更新重启后，Clover 里选择带有“Install”的引导项，不要直接进入系统。