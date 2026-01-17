+++
author = "FlintyLemming"
title = "解决 小马宝莉：西风高地谜团 卡顿问题"
slug = "2eb779ab64688002968ae2d766401545"
date = "2026-01-17"
description = "借助 dxvk 用 Vulcan 启动即可"
categories = ["Game"]
tags = ["小马宝莉"]
image = "https://assets.mitsea.cn/blog/posts/2026/01/%E8%A7%A3%E5%86%B3%20%E5%B0%8F%E9%A9%AC%E5%AE%9D%E8%8E%89%EF%BC%9A%E8%A5%BF%E9%A3%8E%E9%AB%98%E5%9C%B0%E8%B0%9C%E5%9B%A2%20%E5%8D%A1%E9%A1%BF%E9%97%AE%E9%A2%98/My-little-pony-a-zephyr-heights-mystery-key-artfinal.avif"
+++

这游戏优化有问题，隔个两秒左右就会卡一下，导致 Low 帧特别低，操作起来非常痛苦。解决方法非常简单，就是用 dxvk 让游戏使用 Vulkan 运行。效果如下：

![](https://assets.mitsea.cn/blog/posts/2026/01/%E8%A7%A3%E5%86%B3%20%E5%B0%8F%E9%A9%AC%E5%AE%9D%E8%8E%89%EF%BC%9A%E8%A5%BF%E9%A3%8E%E9%AB%98%E5%9C%B0%E8%B0%9C%E5%9B%A2%20%E5%8D%A1%E9%A1%BF%E9%97%AE%E9%A2%98/IMG_0808.avif)

## 具体操作步骤

### 获取游戏目录

就是你安装游戏时选择的那个目录，如果不记得的话，可以按照下面的步骤找到

1. 右键游戏，选择 管理
    
    ![](https://assets.mitsea.cn/blog/posts/2026/01/%E8%A7%A3%E5%86%B3%20%E5%B0%8F%E9%A9%AC%E5%AE%9D%E8%8E%89%EF%BC%9A%E8%A5%BF%E9%A3%8E%E9%AB%98%E5%9C%B0%E8%B0%9C%E5%9B%A2%20%E5%8D%A1%E9%A1%BF%E9%97%AE%E9%A2%98/%E5%9B%BE%E7%89%87.avif)
    
2. 在 文件 选项卡中点击 浏览…
    
    ![](https://assets.mitsea.cn/blog/posts/2026/01/%E8%A7%A3%E5%86%B3%20%E5%B0%8F%E9%A9%AC%E5%AE%9D%E8%8E%89%EF%BC%9A%E8%A5%BF%E9%A3%8E%E9%AB%98%E5%9C%B0%E8%B0%9C%E5%9B%A2%20%E5%8D%A1%E9%A1%BF%E9%97%AE%E9%A2%98/%E5%9B%BE%E7%89%87%201.avif)
    
3. 然后就看到游戏文件夹了
    
    ![](https://assets.mitsea.cn/blog/posts/2026/01/%E8%A7%A3%E5%86%B3%20%E5%B0%8F%E9%A9%AC%E5%AE%9D%E8%8E%89%EF%BC%9A%E8%A5%BF%E9%A3%8E%E9%AB%98%E5%9C%B0%E8%B0%9C%E5%9B%A2%20%E5%8D%A1%E9%A1%BF%E9%97%AE%E9%A2%98/%E5%9B%BE%E7%89%87%202.avif)
    

### 放置 dll 文件

1. 从 dxvk 的仓库下载最新的 Release，并解压
    
    https://github.com/doitsujin/dxvk/releases
    
2. 打开 My Little Pony- A Zephyr Heights Mystery\Content 文件夹，将 d3d11.dll 和 dxgi.dll 这两个文件放到这个 Content 目录里
    
    ![](https://assets.mitsea.cn/blog/posts/2026/01/%E8%A7%A3%E5%86%B3%20%E5%B0%8F%E9%A9%AC%E5%AE%9D%E8%8E%89%EF%BC%9A%E8%A5%BF%E9%A3%8E%E9%AB%98%E5%9C%B0%E8%B0%9C%E5%9B%A2%20%E5%8D%A1%E9%A1%BF%E9%97%AE%E9%A2%98/%E5%9B%BE%E7%89%87%203.avif)
    

至此步骤全部完成，可以打开游戏看看效果了