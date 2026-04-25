+++
author = "FlintyLemming"
title = "单机 H200 HGX Day0 Deepseek v4 Pro 性能浅测"
slug = "34d779ab6468808eb676e337f03ef378"
date = "2026-04-25"
description = "不太能用，需要关注后续多级缓存方案"
categories = ["AI"]
tags = ["H200", "Deepseek"]
image = "https://assets.mitsea.cn/blog/posts/2026/04/%E5%8D%95%E6%9C%BA%20H200%20HGX%20Day0%20Deepseek%20v4%20Pro%20%E6%80%A7%E8%83%BD%E6%B5%85%E6%B5%8B/danielle-suijkerbuijk-wQVXDB17eDc-unsplash.avif"
+++

## 环境配置

| 配置项目 | 详细参数 |
| --- | --- |
| **模型** | DeepSeek-V4-Pro |
| **硬件资源** | 8× NVIDIA H200 (141GB each) |
| **并行策略** | TP (Tensor Parallelism) = 8 |
| **推理引擎** | vLLM (版本: deepseekv4-cu129) |
| **KV Cache** | FP8 精度, block_size=256 |
| **Cache 容量** | ~14,772 blocks (约合 3.78M tokens) |

测试过程和结论整理使用 Claude Code + GLM 5.1 自主完成

## 默认启动参数

```jsx
services:
  deepseek-v4-pro:
    image: vllm/vllm-openai:deepseekv4-cu129
    container_name: deepseek-v4-pro
    privileged: true
    ipc: host
    ports:
      - "30001:8000"
    volumes:
      - ./:/model
      - ./vllm-default/.cache/huggingface:/root/.cache/huggingface
    environment:
      - VLLM_ENGINE_READY_TIMEOUT_S=3600
    deploy:
      resources:
        reservations:
          devices:
            - driver: nvidia
              capabilities: [gpu]
              count: all
    command:
      - /model
      - --trust-remote-code
      - --kv-cache-dtype
      - fp8
      - --block-size
      - "256"
      - --enable-expert-parallel
      - --tensor-parallel-size
      - "8"
      - --max-model-len
      - "800000"
      - --gpu-memory-utilization
      - "0.95"
      - --max-num-seqs
      - "512"
      - --max-num-batched-tokens
      - "512"
      - --no-enable-flashinfer-autotune
      - --compilation-config
      - '{"mode": 0, "cudagraph_mode": "FULL_DECODE_ONLY"}'
      - --tokenizer-mode
      - deepseek_v4
      - --tool-call-parser
      - deepseek_v4
      - --enable-auto-tool-choice
      - --reasoning-parser
      - deepseek_v4
      - --served-model-name
      - deepseek-v4-pro
    restart: unless-stopped
```

## 性能实测数据

### 无 Prefix Cache（冷启动/无缓存）

| Prompt Tokens | TTFT (首字延迟) | Decode (稳定速率) | Completion | Total Time |
| --- | --- | --- | --- | --- |
| 1,196 | 17.63s | 68.33 tok/s | 189 tok | 20.76s |
| 5,198 | 15.25s | 66.12 tok/s | 118 tok | 17.01s |
| 10,197 | 9.99s | 66.10 tok/s | 141 tok | 12.09s |
| 30,187 | 18.23s | 65.52 tok/s | 158 tok | 20.58s |

### 启用 Prefix Cache

*注：以下数据在启用 Prefix Cache 后测得，Completion 固定为 2 tokens（仅测首字延迟）。*

| Prompt Tokens | TTFT (首字延迟) | Completion | Total Time |
| --- | --- | --- | --- |
| 714 | 0.41s | 2 tok | 0.42s |
| 1,397 | 0.41s | 2 tok | 0.42s |
| 2,762 | 0.60s | 2 tok | 0.61s |
| 5,494 | 1.17s | 2 tok | 1.18s |
| 10,954 | 2.13s | 2 tok | 2.13s |
| 21,878 | 4.19s | 2 tok | 4.19s |
| 43,723 | 8.11s | 2 tok | 8.12s |
| 87,414 | 16.14s | 2 tok | 16.14s |
| 131,104 | 16.22s | 2 tok | 16.22s |

## 核心发现

1. **Decode 表现极稳**：生成速度稳定在 **~66 tok/s**，表现出极强的健壮性，基本不受输入上下文长度波动的影响。
2. **TTFT 特征**：
    - 冷启动（系统处理首个请求）约需 **17s**。
    - 后续增量请求的 TTFT 约为 **0.3-0.6s / 1k tokens**。
3. **上下文容量边界**：
    - **无 Prefix Cache**：最大可用上下文约为 **30,000** prompt tokens。
    - **启用 Prefix Cache**：最大可用上下文显著提升至 **131,000** prompt tokens。

## 瓶颈深度分析

测试发现目前核心瓶颈并非显存容量限制，而是 **vLLM 调度器的 Chunked Prefill 准入控制机制** 导致的逻辑冲突。不过剩余显存太少没有充足的 KV Cache 总体上来说仍然是最关键因素。

### 抢占与死循环问题

在设置 `max-num-batched-tokens=512` 且 `max-model-len=800000` 时，长请求极易触发以下链路：

- **Running → Waiting**: 当系统压力或调度触发抢占时，请求被挂起。
- **调度死循环**: 调度器尝试重新激活请求，但受限于准入控制参数，导致请求无法重新获得计算资源，KV Cache 使用率卡在 0%，系统进入死循环。

### 参数调优尝试

针对 `max-num-batched-tokens` 的调整结果如下：

| max-num-batched-tokens | 状态 | KV Cache Blocks | 备注 |
| --- | --- | --- | --- |
| 512 (默认) | OK | 14,772 | 原始配置，~30k (无缓存) / ~131k (有缓存) 触发抢占 |
| 2,048 | OK | 14,712 | KV Cache 几乎无变化，抢占问题仍存在 |
| 4,096 | OK | 14,692 | KV Cache 几乎无变化，50k 附近仍触发抢占 |
| 8,192 | OK | 13,620 | KV Cache 略微下降，抢占问题仍存在 |
| 131,072 | OOM | — | 需额外分配 21 GiB，仅剩 3.75 GiB 可用 |

- **小幅增大 (2048 - 8192)**：KV Cache 总量几乎无变化，但依然无法解决长请求被抢占后的重新调度问题。
- **大幅增大 (131072)**：导致 **OOM (Out of Memory)**。
    - *原因分析*：模型权重已占用约 131 GiB 显存，留给激活缓冲区的空间不足以支撑如此大规模的 batch tokens（需要约 21 GiB）。

### 关键结论

- **调度器局限性**：当前问题是 vLLM 调度器在特定参数组合下的已知限制，即便 KV Cache 仅消耗了 **4%**，请求也会因为调度逻辑而被抢占。
- **Prefix Cache 的价值**：启用 Prefix Cache 后，调度器仅需为新增的少量 Token 分配 KV Cache 和计算资源，巧妙地绕过了 Chunked Prefill 的准入瓶颈，从而使模型能稳定运行在 131k 长度下。

> Photo by [Danielle Suijkerbuijk](https://unsplash.com/@vandaantje?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/photos/delicate-wildflowers-submerged-in-milky-water-wQVXDB17eDc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
      