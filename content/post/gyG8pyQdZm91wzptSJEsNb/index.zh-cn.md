+++
author = "FlintyLemming"
title = "AMD AI MAX+ 395 小主机初期体验"
slug = "gyG8pyQdZm91wzptSJEsNb"
date = "2025-06-12"
description = "性价比有限的AI小主机体验"
categories = ["Consumer"]
tags = ["AMD", "ROCm", "AI"]
image = "https://assets.mitsea.cn/blog/posts/2025/06/AMD%20AI%20MAX+%20395%20%E5%B0%8F%E4%B8%BB%E6%9C%BA%E5%88%9D%E6%9C%9F%E4%BD%93%E9%AA%8C/matthew-stephenson-HHhDlTqwPfk-unsplash.avif"
+++

## 背景

公司在尝试一些小规模推理的一些应用场景，顺便试试 Linux 做开发环境，所以打算买两个小主机试试效果，一个 Nvidia DGX Spark，这个还没货，还有一个就是这个 AMD AI MAX+ 395 小主机了。

关于小模型推理时常被人调侃部署了个人工智障，但是从接触到的客户和已经做了的项目来看，有的客户他就只有基本的翻译、总结、RAG 等需求，并不是拿来当成 ChatGPT 问任何问题，所以一个低预算部署小模型的方案还是有需求的（虽然我觉得这个 AMD 的方案预算并不是很低了，价格贵大概是因为这个芯片本身的制造成本太高了）。

其实买这个之前其实比较纠结，因为 AMD 的 ROCm 加速环境听说一直都比 CUDA 用起来要麻烦得多，但是 AMD 官方各种宣传，有官方背书倒是也不至于完全不能用.jpg

## 设备概况

买的是磐镭的 YO1，本来是想买极摩客的，因为那个是 AMD 官方背书的，而且质感稍微好点。不过据说都是同样的公版方案，而且公司采购要求必须买京东自营的，所以就买了磐镭的。到手确实质感比较一般，属于是凑合能用，观众如果要买还是买极摩客吧。

机器到手预装 Windows 11 专业版，显存内存默认对半分，各 64GB。2T 硬盘我分了一部分装了 Ubuntu，开机时选择进入的系统。

机器本身性能就没啥好测的了，我帖几张图在这里，基本就是 CPU 约等于 Ultra 7 265K，GPU 约等于 4060 桌面版。对于核显来说确实很强，但是 15000 的价格我觉得还是太贵了，这价格都够买个 5070Ti 游戏本了。

**CPU-Z**，新版已经适配了这个平台

![](https://assets.mitsea.cn/blog/posts/2025/06/AMD%20AI%20MAX+%20395%20%E5%B0%8F%E4%B8%BB%E6%9C%BA%E5%88%9D%E6%9C%9F%E4%BD%93%E9%AA%8C/image_ZO1KiNZD5e.avif)

![](https://assets.mitsea.cn/blog/posts/2025/06/AMD%20AI%20MAX+%20395%20%E5%B0%8F%E4%B8%BB%E6%9C%BA%E5%88%9D%E6%9C%9F%E4%BD%93%E9%AA%8C/image_2qY_kuNI--.avif)

**GPU-Z**

![](https://assets.mitsea.cn/blog/posts/2025/06/AMD%20AI%20MAX+%20395%20%E5%B0%8F%E4%B8%BB%E6%9C%BA%E5%88%9D%E6%9C%9F%E4%BD%93%E9%AA%8C/image_ybVTzeuYwH.avif)

**Cinebench 2024**，多核心差点没打过 Apple M1 Ultra，有点丢人，单核也没打过 M1

![](https://assets.mitsea.cn/blog/posts/2025/06/AMD%20AI%20MAX+%20395%20%E5%B0%8F%E4%B8%BB%E6%9C%BA%E5%88%9D%E6%9C%9F%E4%BD%93%E9%AA%8C/image_KK96dCMetG.avif)

![](https://assets.mitsea.cn/blog/posts/2025/06/AMD%20AI%20MAX+%20395%20%E5%B0%8F%E4%B8%BB%E6%9C%BA%E5%88%9D%E6%9C%9F%E4%BD%93%E9%AA%8C/image_M3ZeNqY-tv.avif)

**3DMark Time Spy Extreme**，GPU 约等于桌面端 4060。

![](https://assets.mitsea.cn/blog/posts/2025/06/AMD%20AI%20MAX+%20395%20%E5%B0%8F%E4%B8%BB%E6%9C%BA%E5%88%9D%E6%9C%9F%E4%BD%93%E9%AA%8C/image_dpeAiB5Lvv.avif)

**AIDA64 Memory Benchmark**，不知道为啥明明没开 Hyper-V 和虚拟化平台，还是提示说开了 Hypervisor。就是这个内存带宽有点捉急啊，之前别人还在嘲讽 DGX Spark 内存带宽只有 273 GB/s，这还不如 DGX Spark 呢

![](https://assets.mitsea.cn/blog/posts/2025/06/AMD%20AI%20MAX+%20395%20%E5%B0%8F%E4%B8%BB%E6%9C%BA%E5%88%9D%E6%9C%9F%E4%BD%93%E9%AA%8C/photo_2025-06-11%2021.09.07_HHGriLsv3h.avif)

## LLM 推理概况

#### vLLM

ROCm 是有一个在维护的 [vLLM](https://github.com/ROCm/vllm "vLLM")，然后他有一个现成的 docker [镜像](https://hub.docker.com/layers/rocm/vllm-dev/rocm6.4.1_navi_ubuntu24.04_py3.12_pytorch_2.7_vllm_0.8.5/images/sha256-ec755ba40711566c70f8279ceb0cb92b7d42a81448f64e59ea8ce1e1199269f8 "镜像")，但是跑不起来，在提 [issue](https://github.com/ROCm/ROCm/issues/4909 "issue")

#### SGLang

SGLang 实际上是 vLLM 的分支，在 ROCm 上用 SGLang 就是以 vLLM 的 docker 镜像为 base，所以 vLLM 那个跑不了的话，这个肯定也跑不了

#### Ollama

根据[官方文档](https://github.com/ollama/ollama/blob/main/docs/gpu.md#amd-radeon "官方文档")，Ollama 还没支持到这个 GPU

#### LM Studio (llama.cpp)

LM Studio 的话，后端其实调用的是 llama.cpp，用的 Vulkan GPU 加速，而不是 ROCm。不过这并不代表效率就会比 ROCm 低，因为社区是有人反应说 AMD 消费端 GPU这个 ROCm 在部分场景里的加速效果是不如 Vulkan 的。

Vulkan 加速其实是 AMD 对于这个 AI MAX+ 平台推广时推荐的平台，也是最成熟的平台，所以不出所料模型很容易就可以加载并进行推理。

这边推理一个 DeepSeek-R1-0528-Qwen3-8B 的 GGUF 8bit 模型也是完全没问题，就是这个速度并不算快啊，后面有单独的速度测试。

![](https://assets.mitsea.cn/blog/posts/2025/06/AMD%20AI%20MAX+%20395%20%E5%B0%8F%E4%B8%BB%E6%9C%BA%E5%88%9D%E6%9C%9F%E4%BD%93%E9%AA%8C/image_RI4ewo-ZvE.avif)

但是 LM Studio 有个很大的问题是**不支持并发**，同时发送两个请求，另一个只能排队等待，导致只适合单线程任务使用场景。

## LLM 推理简析

### 模型选择

模型选择主要是围绕着这 96GB 显存和能接受的推理速度来选择，对于这样一台配备大显存但是推理速度又比较一般的机器，MoE 模型更加适合，因为 MoE 模型加载的时候需要把所有参数加载到显存中，但是推理时调用的参数量又很小。目前测试选择了以下几个参考模型：

lmstudio-community/DeepSeek-R1-0528-Qwen3-8B-Q8\_0

unsloth/Qwen3-30B-A3B-128K-UD-Q8\_K\_XL

unsloth/Qwen3-32B-128K-UD-Q8\_K\_XL

unsloth/Qwen3-235B-A22B-128K-UD-Q2\_K\_XL

由于 Vulkan 加速平台 llama.cpp 的限制，模型基本都选用 unsloth 动态量化 2.0 的模型，虽然说是量化模型，但是从 unsloth 的[文章](https://docs.unsloth.ai/basics/unsloth-dynamic-2.0-ggufs "文章")可以看到对于一个原生 fp8 模型来说 Q6 Q8 的 UD 2.0 模型 MMLU 都是等于原始模型的，从效果上来看可以看作是没有损失。

![](https://assets.mitsea.cn/blog/posts/2025/06/AMD%20AI%20MAX+%20395%20%E5%B0%8F%E4%B8%BB%E6%9C%BA%E5%88%9D%E6%9C%9F%E4%BD%93%E9%AA%8C/image_3pXwBIvlOe.avif)

### 性能

**lmstudio-community/DeepSeek-R1-0528-Qwen3-8B-Q8\_0**

大约是 30 tokens 每秒，然后 Flash Attention 是支持这个模型的，打开后可以顺利开出 128k 上下文，后面测试默认也都开启

![](https://assets.mitsea.cn/blog/posts/2025/06/AMD%20AI%20MAX+%20395%20%E5%B0%8F%E4%B8%BB%E6%9C%BA%E5%88%9D%E6%9C%9F%E4%BD%93%E9%AA%8C/image_TkY5IURxEV.avif)

**unsloth/Qwen3-30B-A3B-128K-UD-Q8\_K\_XL**

大约是 30 tokens 每秒，MoE 模型小激活量的优势在这里体现出来了，做到了 30B 的模型推理速度与 8B 模型速度一致。同样能开出来 128k 上下文。

**unsloth/Qwen3-32B-128K-UD-Q8\_K\_XL**

硬推 32B 8bit 就慢了，只有 7 tokens 每秒，所以还是不建议拿这个机器跑非 MoE 模型，大一点点都比较费劲，128k 上下文是可以开出来的。

**unsloth/Qwen3-235B-A22B-128K-UD-Q2\_K\_XL**

大约 20 tokens 每秒，其实速度还行，但是感觉实际意义不是很大？因为上下文基本上开不了多大了，属于是探查一下 96G 显存能够跑的模型的上限

![](https://assets.mitsea.cn/blog/posts/2025/06/AMD%20AI%20MAX+%20395%20%E5%B0%8F%E4%B8%BB%E6%9C%BA%E5%88%9D%E6%9C%9F%E4%BD%93%E9%AA%8C/image_KQHnwJkSBw.avif)

所以情势就比较明朗了，目前比较适合这个机器的模型是 **unsloth/Qwen3-30B-A3B-128K-UD-Q8\_K\_XL**，而且 Qwen3 可以控制是否思考，也支持 function calling，实用性还是比较不错的。

### 横向比较

总的来说，这个机器的纯推理性能真的算不上强，大概是还没优化明白，也有可能是内存带宽不大的原因。那么此时就有一个问题了，倘若我掏出来 500GB/s 内存带宽的大机器出来，用 CPU 纯硬跑能不能干得过他呢

![](https://assets.mitsea.cn/blog/posts/2025/06/AMD%20AI%20MAX+%20395%20%E5%B0%8F%E4%B8%BB%E6%9C%BA%E5%88%9D%E6%9C%9F%E4%BD%93%E9%AA%8C/image_-oz653hV64.avif)

我们直接加载相同 32B 模型直接进内存，拉满 128k 上下文，毕竟这台机器内存不仅带宽够大，容量也有 2TB，完全不慌

![](https://assets.mitsea.cn/blog/posts/2025/06/AMD%20AI%20MAX+%20395%20%E5%B0%8F%E4%B8%BB%E6%9C%BA%E5%88%9D%E6%9C%9F%E4%BD%93%E9%AA%8C/image_qukRCYWOWT.avif)

很可惜的是，速度并不快，推理 **unsloth/Qwen3-32B-128K-UD-Q8\_K\_XL** 模型仅仅只有每秒 5 tokens，还没打过 AMD 小主机

![](https://assets.mitsea.cn/blog/posts/2025/06/AMD%20AI%20MAX+%20395%20%E5%B0%8F%E4%B8%BB%E6%9C%BA%E5%88%9D%E6%9C%9F%E4%BD%93%E9%AA%8C/image_c82pcUC2wU.avif)

此时 CPU 吃满 96 线程（试过超过物理核心数反而会变慢），内存带宽达到了 180GB/s 左右

![](https://assets.mitsea.cn/blog/posts/2025/06/AMD%20AI%20MAX+%20395%20%E5%B0%8F%E4%B8%BB%E6%9C%BA%E5%88%9D%E6%9C%9F%E4%BD%93%E9%AA%8C/image_uIJXOJN1RO.avif)

看起来推理 LLM 模型还是必须要一个加速硬件

### Apple Silicon

但是，此时如果我掏出来 Apple Silicon 呢，既然目前能跑的模型也就是 30B 左右，如果掏出来一个 48G 或者 64G 内存的 Mac mini，AMD 小主机的性价比优势是否就不复存在了呢？

![](https://assets.mitsea.cn/blog/posts/2025/06/AMD%20AI%20MAX+%20395%20%E5%B0%8F%E4%B8%BB%E6%9C%BA%E5%88%9D%E6%9C%9F%E4%BD%93%E9%AA%8C/image_EOnQiBPeWS.avif)

行.jpg M4 Pro 推理 32B 8bit 模型根据[别人的测试](https://www.53ai.com/news/zhinengyingjian/2025041267419.html "别人的测试")，似乎跟 AMD 小主机半斤八两。考虑到 M4 Pro 即便是选配 64G 内存 + 512G 存储都要 15499，看起来 AMD 小主机的性价比多少还是有一点。

## ROCm 简单测试

AMD 在 2025 Computex 台北电脑展上官宣了 ROCm 对于 Ryzen AI MAX 平台的支持

![](https://assets.mitsea.cn/blog/posts/2025/06/AMD%20AI%20MAX+%20395%20%E5%B0%8F%E4%B8%BB%E6%9C%BA%E5%88%9D%E6%9C%9F%E4%BD%93%E9%AA%8C/image_HySNpWDfKv.avif)

不过就我现在的体验来说，ROCm 最新 6.4.1 版本还没完全支持 Ryzen AI MAX 平台，仅仅是能正常安装并且能看到设备 agent，我尝试使用 `rocm/pytorch:rocm6.4.1_ubuntu24.04_py3.12_pytorch_release_2.6.0` 这个镜像来执行一个简单的 pytorch 脚本都会报 `invalid device function` 的错误

![](https://assets.mitsea.cn/blog/posts/2025/06/AMD%20AI%20MAX+%20395%20%E5%B0%8F%E4%B8%BB%E6%9C%BA%E5%88%9D%E6%9C%9F%E4%BD%93%E9%AA%8C/image_0Zr5JELMHd.avif)

所以感觉 ROCm 这块感觉只能战未来了

## 绘图相关

由于 ROCm 还没适配明白，所以根据 AMD 的[文档](https://rocm.docs.amd.com/projects/radeon/en/latest/docs/advanced/comfyui/installcomfyui.html "文档")，ComfyUI 肯定是用不了了。Windows 下的话，可以用 [https://www.amuse-ai.com/](https://www.amuse-ai.com/ "https://www.amuse-ai.com/") 来跑 SDXL 模型，Amuse AI 看起来是 AMD 在 Windows 下专门推理绘图模型的工具，大大的 AMD Partner logo 似乎说明了一切。

![](https://assets.mitsea.cn/blog/posts/2025/06/AMD%20AI%20MAX+%20395%20%E5%B0%8F%E4%B8%BB%E6%9C%BA%E5%88%9D%E6%9C%9F%E4%BD%93%E9%AA%8C/image_733GBwKmkw.avif)

绘图这块笔者研究的就不多了，跑一个 AMD 自己微调的 SDXL io32 模型，8步生成一个 1024x1024 的图片速度大概是 5.5 秒

![](https://assets.mitsea.cn/blog/posts/2025/06/AMD%20AI%20MAX+%20395%20%E5%B0%8F%E4%B8%BB%E6%9C%BA%E5%88%9D%E6%9C%9F%E4%BD%93%E9%AA%8C/image_8WOsSWDv7Q.avif)

## 小结

总的来说，AMD 这个 AI MAX 平台性价比不是很高，但别家确实难找到对位产品，DGX Spark 的价格又比它高得多。对于单人或者单线程简单 LLM 推理场景有其自身优势，在我接触到的 LLM 项目中，确有适合他的应用场景。

ROCm 支持不佳，只能等后续更新，不过 AMD 对 AI MAX+ 这个新平台未来的推崇程度尚不明晰，个人不建议选择任何期货电子产品，购买设备前还是要根据项目实际需要来判断是否适合。

> Photo by [Matthew Stephenson](https://unsplash.com/@matthewryanstephenson?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/snowy-mountain-peak-shrouded-in-a-dark-ominous-sky-HHhDlTqwPfk?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)