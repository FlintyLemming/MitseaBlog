+++
author = "FlintyLemming"
title = "本地部署 DeepSeek-V3-0324"
slug = "1bc7bda595c580dd9b1bdd5842370b9a"
date = "2025-03-26"
description = "又到一个大玩具"
categories = ["Consumer", "Linux"]
tags = ["DeepSeek", "Nvidia"]
image = "https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2025/03/%E6%9C%AC%E5%9C%B0%E9%83%A8%E7%BD%B2%20DeepSeek-V3-0324/HGX%20H200%20Tech%20Blog.avif"
+++

## 前言

继先前的 [Dell R750xa + A6000](https://blog.mitsea.com/d29bb28b14984443b232263348b946ba/) 后，又到了个大玩具，事 Dell XE9680 + H200.jpg

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2025/03/%E6%9C%AC%E5%9C%B0%E9%83%A8%E7%BD%B2%20DeepSeek-V3-0324/image.avif)

机器到了有段时间了，但之所以拖到现在写（水）文章是因为目前开源推理框架对于 DeepSeek 终于算是可用了，特别是在 128k 上下文这块。所以这篇文章虽然看着非常水，但先前也是与 vLLM 和 SGLang 斗智斗勇了数个深夜。

目前的关键更新是 vLLM 在 0.8 版本中对于 DeepSeek 推理默认使用 v1 版本的分支了，所以推理后端默认开始使用 FlashMLA 了，这有效解决了先前 vLLM 使用 TritonMLA 后端导致莫名的上下文不能超过 32k。

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2025/03/%E6%9C%AC%E5%9C%B0%E9%83%A8%E7%BD%B2%20DeepSeek-V3-0324/image%201.avif)

至于 SGLang，在使用 EP + FlashMLA 时，如果提示词比较大（比如 70k），那解码会非常缓慢，这导致等待首字可能长达两分钟，原因未知。不过 SGLang 感觉好像也是主要搞 AMD ROCm 推理的，我们这 CUDA 就不凑热闹了。

## 机器准备

1. 由于 Nvidia 这几年打算逐步开源驱动，先从计算卡开始，所以 H200 官方的驱动默认就推荐你安装开源版本。既然官方都有开源版本了， nouveau 没有 H200 的开源驱动，所以从官网下载驱动直接装就行，也不需要先卸载什么驱动。
2. 由于 H200 是 nvswitch 互联的显卡，后续应用程序依赖一个叫做 fabric manager 的东西，这个东西的版本需要与驱动版本严格匹配，但是他有的时候会晚于驱动发布，所以你要先看好这个东西的版本最新是多少，然后下载对应版本的驱动，否则就会报下面这个错误
    
    ```bash
    nv-fabricmanager[131760]: fabric manager NVIDIA GPU driver interface version 570.86.15 don't match with driver version 570.124.06. Please update with matching NVIDIA driver package.
    ```
    
3. 正常安装新版驱动即可，感觉高版本 CUDA（目前是 12.8）是向下兼容的。
4. 下载好 DeepSeek-V3-0324 模型 checkpoint 和配置文件。

## 具体步骤

vLLM 1.0 的逻辑会改成 SGLang 那种，就是推荐你用 Docker 安装，然后你进到容器里面的 bash 操作。不过目前版本还没改，我这里就直接弄个 conda 执行了。

1. 使用 conda 创建一个新的 python 3.12 环境并正常安装 vLLM 即可
    
    ```bash
    pip install vllm
    ```
    
2. 切换到 vllm 环境中并直接执行如下命令即可，新版非常简单，不需要加任何限制长度的参数，默认开的就是 128k 上下文
    
    ```bash
    vllm serve /path/to/your/models/file/DeepSeek-V3-0324 \
    --tensor-parallel-size 8 \
    --load-format auto \
    --trust-remote-code \
    --served-model-name DeepSeek-V3-0324 \
    --port 8000 \
    --gpu-memory-utilization 0.96
    ```
    

## 测试

可以看到 128k 上下文毫无压力.jpg

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2025/03/%E6%9C%AC%E5%9C%B0%E9%83%A8%E7%BD%B2%20DeepSeek-V3-0324/image%202.avif)

这么长的上下文，速度也是非常不错，比 SGLang 快多了

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2025/03/%E6%9C%AC%E5%9C%B0%E9%83%A8%E7%BD%B2%20DeepSeek-V3-0324/image%203.avif)

官方 API 目前还是 64k 上下文，这下秒杀官方了.jpg

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2025/03/%E6%9C%AC%E5%9C%B0%E9%83%A8%E7%BD%B2%20DeepSeek-V3-0324/image%204.avif)