+++
author = "FlintyLemming"
title = "【归档】龙族手游安卓版开启 60fps"
slug = "cd6c0ed0249d47c39ee2743e87dd7ad1"
date = "2020-05-31"
description = ""
categories = ["Game"]
tags = ["手游", "龙族"]
image = "https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/05/%E9%BE%99%E6%97%8F%E6%89%8B%E6%B8%B8%E5%AE%89%E5%8D%93%E7%89%88%E5%BC%80%E5%90%AF%2060fps/title.avif"
+++

最近这游戏挺火的，凑热闹来试玩了一下，不咋好玩感觉。不过还是研究了怎么开60fps，思路其实还蛮简单，就是搜一下游戏数据文件的内容，关于 framerate 什么的关键词啥的。下面介绍一下方法，不需要 root。

1. 确保你自带的文件管理器有一个能编辑任何格式的文件编辑器，如果没有，建议使用 ES文件浏览器，不过这软件有点小流氓，注意权限。
2. 定位到 /storage/emulated/0（也就是内部存储）/Android/data/co.tencent.lzhx/files/userdata
3. 使用文本编辑器编辑“usercfg.lua”这个文件
4. 打开后搜索（SE文本编辑器的话，在右上角三个点里）“HighFrame”，可以找到“QC_HighFrameRateFlag”这项值
5. 将它改为2，保存。（ES文本编辑器的话，要先点一下左上角的编辑才能修改内容）
    
    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/05/%E9%BE%99%E6%97%8F%E6%89%8B%E6%B8%B8%E5%AE%89%E5%8D%93%E7%89%88%E5%BC%80%E5%90%AF%2060fps/1.avif)
    
6. 回到游戏里就能看到自动弹出的使用60fps的提醒。
    
    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/05/%E9%BE%99%E6%97%8F%E6%89%8B%E6%B8%B8%E5%AE%89%E5%8D%93%E7%89%88%E5%BC%80%E5%90%AF%2060fps/2.avif)