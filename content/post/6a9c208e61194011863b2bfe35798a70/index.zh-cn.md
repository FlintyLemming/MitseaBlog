+++
author = "FlintyLemming"
title = "Windows 11 解决搜不到终端的问题"
slug = "6a9c208e61194011863b2bfe35798a70"
date = "2022-09-10"
description = ""
categories = ["Windows"]
tags = ["Windows 11"]
image = "https://assets.mitsea.cn/blog/posts/2022/09/Windows%2011%20%E8%A7%A3%E5%86%B3%E6%90%9C%E4%B8%8D%E5%88%B0%E7%BB%88%E7%AB%AF%E7%9A%84%E9%97%AE%E9%A2%98/title.avif"
+++

## 问题

中文版 Windows 11 的 Windows Terminal 因为程序名叫“终端”，所以无论是自带的搜索还是 PowerToys Run 都无法通过 “Terminal” 搜到这个程序。

![](https://assets.mitsea.cn/blog/posts/2022/09/Windows%2011%20%E8%A7%A3%E5%86%B3%E6%90%9C%E4%B8%8D%E5%88%B0%E7%BB%88%E7%AB%AF%E7%9A%84%E9%97%AE%E9%A2%98/1.avif)

![](https://assets.mitsea.cn/blog/posts/2022/09/Windows%2011%20%E8%A7%A3%E5%86%B3%E6%90%9C%E4%B8%8D%E5%88%B0%E7%BB%88%E7%AB%AF%E7%9A%84%E9%97%AE%E9%A2%98/2.avif)

## 解决方法

本质就是想办法把 Windows Terminal 的 exe 固定到开始菜单，这样就可以搜索到了。

1. 打开 Windows Terminal 后，在任务管理器中找到它的进程，右键 - 打开文件所在位置
    
    ![](https://assets.mitsea.cn/blog/posts/2022/09/Windows%2011%20%E8%A7%A3%E5%86%B3%E6%90%9C%E4%B8%8D%E5%88%B0%E7%BB%88%E7%AB%AF%E7%9A%84%E9%97%AE%E9%A2%98/3.avif)
    
2. 找到它后，右键 - 固定到“开始”屏幕
    
    ![](https://assets.mitsea.cn/blog/posts/2022/09/Windows%2011%20%E8%A7%A3%E5%86%B3%E6%90%9C%E4%B8%8D%E5%88%B0%E7%BB%88%E7%AB%AF%E7%9A%84%E9%97%AE%E9%A2%98/4.avif)
    
3. 默认就是一个名为 WindowsTerminal 的快捷方式，没有空格，可以右键 - 打开文件所在位置自己改下名字
    
    ![](https://assets.mitsea.cn/blog/posts/2022/09/Windows%2011%20%E8%A7%A3%E5%86%B3%E6%90%9C%E4%B8%8D%E5%88%B0%E7%BB%88%E7%AB%AF%E7%9A%84%E9%97%AE%E9%A2%98/5.avif)
    
4. 这样就可以搜索到了
    
    ![](https://assets.mitsea.cn/blog/posts/2022/09/Windows%2011%20%E8%A7%A3%E5%86%B3%E6%90%9C%E4%B8%8D%E5%88%B0%E7%BB%88%E7%AB%AF%E7%9A%84%E9%97%AE%E9%A2%98/6.avif)
    
Photo by [Viktor Mindt](https://unsplash.com/@vikomio77?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
  