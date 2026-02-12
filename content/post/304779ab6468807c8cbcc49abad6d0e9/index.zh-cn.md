+++
author = "FlintyLemming"
title = "LLM 网页浏览翻译能力浅测"
slug = "304779ab6468807c8cbcc49abad6d0e9"
date = "2026-02-12"
description = "非常随便的测试，结论是 K2 Instruct 最好"
categories = ["AI"]
tags = ["沉浸式翻译"]
image = "https://assets.mitsea.cn/blog/posts/2026/02/LLM%20%E7%BD%91%E9%A1%B5%E6%B5%8F%E8%A7%88%E7%BF%BB%E8%AF%91%E8%83%BD%E5%8A%9B%E6%B5%85%E6%B5%8B/mike-hindle-UxW63Tbtyjk-unsplash.avif"
+++

## 背景

最近想提高下沉浸式翻译的翻译质量，原来一直用的 Qwen3 30B A3B 的模型翻译，其实也够用，但是想看看换别的模型有没有什么提升。但是不知道为什么互联网上评测模型翻译的体验内容很少，所以我就自己简单测一下。测试对象是下面这篇 Reddit 帖子，工具使用 沉浸式翻译+自带 Reddit 翻译提示词

[https://www.reddit.com/r/DeathStranding/comments/1r02cve/issue_death_stranding_directors_cut_pc_game_pass/?sort=new](https://www.reddit.com/r/DeathStranding/comments/1r02cve/issue_death_stranding_directors_cut_pc_game_pass/?sort=new)

然后还有一点是我对速度有要求，所以不接受思考模型，如果测了思考模型我都是把思考关闭了的

## 省流结果

与现在用的 Qwen3 30B A3B 相比，更大的 Qwen3 系列模型并不会带来更好的翻译效果。从结果上来看，翻译效果好坏似乎与模型体积几乎没有关系。考虑到 GPT 诞生之初本来就是为翻译服务，当时还是一个很小的模型，所以可能对于翻译来说，参数量大小并不是一个决定性因素。

综合来说 Kimi K2 Instruct 0905 效果最好，但是输出比较贵，￥16.00/M，如果 Kimi K2 系列能有一个小模型，那么性价比可能会相当高。

以及这个结果与 [https://bench.opensakura.com/](https://bench.opensakura.com/) 的测试也比较相近。

## 详细结果

### Kimi K2 Instruct 0905

效果最好，而且非常地道。只有划线一处翻译的并不是很好，直接翻译为“正在修复中”会比较合适。

![](https://assets.mitsea.cn/blog/posts/2026/02/LLM%20%E7%BD%91%E9%A1%B5%E6%B5%8F%E8%A7%88%E7%BF%BB%E8%AF%91%E8%83%BD%E5%8A%9B%E6%B5%85%E6%B5%8B/%E5%9B%BE%E7%89%87.avif)

![](https://assets.mitsea.cn/blog/posts/2026/02/LLM%20%E7%BD%91%E9%A1%B5%E6%B5%8F%E8%A7%88%E7%BF%BB%E8%AF%91%E8%83%BD%E5%8A%9B%E6%B5%85%E6%B5%8B/%E5%9B%BE%E7%89%87%201.avif)

### Qwen3 30B A3B Instruct 2507

有一处翻译错误，原文是  a patch on the user’s end，意思是得更新客户端才可以解决问题，并不是用户自己打补丁。这个翻译问题一直贯穿到 Qwen3 Max

![](https://assets.mitsea.cn/blog/posts/2026/02/LLM%20%E7%BD%91%E9%A1%B5%E6%B5%8F%E8%A7%88%E7%BF%BB%E8%AF%91%E8%83%BD%E5%8A%9B%E6%B5%85%E6%B5%8B/%E5%9B%BE%E7%89%87%202.avif)

![](https://assets.mitsea.cn/blog/posts/2026/02/LLM%20%E7%BD%91%E9%A1%B5%E6%B5%8F%E8%A7%88%E7%BF%BB%E8%AF%91%E8%83%BD%E5%8A%9B%E6%B5%85%E6%B5%8B/%E5%9B%BE%E7%89%87%203.avif)

### Qwen3 Next 80B A3B

![](https://assets.mitsea.cn/blog/posts/2026/02/LLM%20%E7%BD%91%E9%A1%B5%E6%B5%8F%E8%A7%88%E7%BF%BB%E8%AF%91%E8%83%BD%E5%8A%9B%E6%B5%85%E6%B5%8B/%E5%9B%BE%E7%89%87%204.avif)

![](https://assets.mitsea.cn/blog/posts/2026/02/LLM%20%E7%BD%91%E9%A1%B5%E6%B5%8F%E8%A7%88%E7%BF%BB%E8%AF%91%E8%83%BD%E5%8A%9B%E6%B5%85%E6%B5%8B/%E5%9B%BE%E7%89%87%205.avif)

### Qwen3 Max

![](https://assets.mitsea.cn/blog/posts/2026/02/LLM%20%E7%BD%91%E9%A1%B5%E6%B5%8F%E8%A7%88%E7%BF%BB%E8%AF%91%E8%83%BD%E5%8A%9B%E6%B5%85%E6%B5%8B/%E5%9B%BE%E7%89%87%206.avif)

![](https://assets.mitsea.cn/blog/posts/2026/02/LLM%20%E7%BD%91%E9%A1%B5%E6%B5%8F%E8%A7%88%E7%BF%BB%E8%AF%91%E8%83%BD%E5%8A%9B%E6%B5%85%E6%B5%8B/%E5%9B%BE%E7%89%87%207.avif)

### GLM 4.7 Thinking Disabled

效果不好，读起来不通顺切奇怪，看起来 GLM 并不适合拿来翻译

![](https://assets.mitsea.cn/blog/posts/2026/02/LLM%20%E7%BD%91%E9%A1%B5%E6%B5%8F%E8%A7%88%E7%BF%BB%E8%AF%91%E8%83%BD%E5%8A%9B%E6%B5%85%E6%B5%8B/%E5%9B%BE%E7%89%87%208.avif)

![](https://assets.mitsea.cn/blog/posts/2026/02/LLM%20%E7%BD%91%E9%A1%B5%E6%B5%8F%E8%A7%88%E7%BF%BB%E8%AF%91%E8%83%BD%E5%8A%9B%E6%B5%85%E6%B5%8B/%E5%9B%BE%E7%89%87%209.avif)

## DeepSeek V3.2 Thinking Disabled

跟 GLM 坐一桌

![](https://assets.mitsea.cn/blog/posts/2026/02/LLM%20%E7%BD%91%E9%A1%B5%E6%B5%8F%E8%A7%88%E7%BF%BB%E8%AF%91%E8%83%BD%E5%8A%9B%E6%B5%85%E6%B5%8B/%E5%9B%BE%E7%89%87%2010.avif)

![](https://assets.mitsea.cn/blog/posts/2026/02/LLM%20%E7%BD%91%E9%A1%B5%E6%B5%8F%E8%A7%88%E7%BF%BB%E8%AF%91%E8%83%BD%E5%8A%9B%E6%B5%85%E6%B5%8B/%E5%9B%BE%E7%89%87%2011.avif)

> Photo by [Mike Hindle](https://unsplash.com/@mikehindle?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/photos/close-up-of-a-dark-palm-leaf-with-radiating-lines-UxW63Tbtyjk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
      