+++
author = "FlintyLemming"
title = "Qwen3.8-27B 部署技术报告"
slug = "3c1779ab6468800eac67f555a5301e82"
date = "2026-08-19"
description = "最终驻留配置为 FP8 + TP2 + DFlash2，单流 96 tok/s"
categories = ["AI"]
tags = ["Qwen", "SGLang"]
image = "https://assets.flinty.moe/blog/posts/2026/08/Qwen3%208-27B%20%E9%83%A8%E7%BD%B2%E6%8A%80%E6%9C%AF%E6%8A%A5%E5%91%8A/pawel-czerwinski-KMICAjAH7fU-unsplash.jpg"
+++

- 硬件：4× NVIDIA RTX A6000 48GB（Ampere sm_86，无原生 FP8 Tensor Core）
- 服务框架：SGLang（镜像 `lmsysorg/sglang:qwen38-27b`），OpenAI 兼容 API

---

## 一、选型变迁

部署共经历四个阶段，目标从"先跑起来"逐步演进为"更快、更省卡"。

### 阶段 1：BF16 + TP4 + MTP（基线）

- 模型：`Qwen3.8-27B` 原始 BF16 权重（约 52 GB），4 卡 TP=4。
- 投机解码：模型自带 MTP 头，EAGLE 模式（steps=3, topk=1, draft tokens=4）。
- 结果：单流 decode 73.4 tok/s。权重占满 4 卡，KV 池 69 万 token。
- 问题：27B BF16 权重使 decode 强内存带宽受限，4 卡全被占用。

### 阶段 2：FP8 + TP4 + MTP（量化提速）

- 换用 FP8 量化权重 `Qwen3.8-27B-FP8`（约 29 GB，fp8 e4m3 block-128，`lm_head` 保持 BF16）。
- 关键点：A6000 是 Ampere，没有 FP8 Tensor Core，SGLang 自动走 **Marlin weight-only FP8（W8A16）**——权重存 FP8、计算 BF16。收益来自权重从每卡约 38 GB 降到约 7.6 GB，decode 读带宽大幅下降。
- 结果：单流 decode 109.1 tok/s（**+49%**），KV 池增至 89.7 万 token。**这是全部配置中最快的一档**，但仍占 4 卡。

### 阶段 3：FP8 + TP2 + MTP（省卡，缩到 GPU 2+3）

- 应需求把服务收缩到物理 GPU 2+3，TP=2，释放 GPU 0+1 给其他任务。
- 结果：单流 decode 76.4 tok/s。每卡权重翻倍（约 15 GB）、单步计算量增大，比 TP4 慢，但仍略快于 4 卡 BF16 基线。
- 此为"驻留配置"的第一版。

### 阶段 4：FP8 + TP2 + DFlash2（当前驻留配置）

- 用 **DFlash2 草稿模型**（[z-lab/Qwen3.8-27B-DFlash2](https://huggingface.co/z-lab/Qwen3.8-27B-DFlash2)，3.8 GB）替换 MTP 投机解码：`-speculative-algorithm=DFLASH`，8 个 draft token。DFLASH 与 MTP/EAGLE 互斥，是替换而非叠加。
- 两个部署约束：
    1. **镜像**：DFlash2 支持是 SGLang PR 35371，2026-08-19 才合入，Docker Hub 最新 nightly（08-18）还没包含。因此在 `qwen38-27b` 镜像上打了本地 overlay 包 `sglang-qwen38-27b-dflash2:local`（从合入 commit 拷贝 4 个文件，另做了 3 处 qwen38 分支兼容性修补）。
    2. **不能独立放 GPU 0**：DFlash2 需要注入目标模型第 5/19/33/47/61 层的 hidden states，草稿 worker 与目标 TP rank 同卡运行，必须与目标模型共置在 GPU 2+3。
- 结果：单流 decode 96.1 tok/s（比同卡 MTP **+26%**），accept length 约 3.3–3.6 / 8。代价是 conc8 聚合吞吐 356 → 227（verify CUDA graph 只采集到 bs≤7），KV 池 31.5 万 → 24.6 万 token（草稿模型占了同卡显存）。

### 草稿模型的放置与并行方式

草稿模型与目标模型**共置在同一组卡上，并随目标一起做 TP2 切分**，这也是 hidden-state 注入式草稿的标准跑法：

- **共置**：EAGLE、MTP、DFlash 这类草稿都要消费目标模型的中间层 hidden states（DFlash2 是第 5/19/33/47/61 层），每个 decode step 都要传一次。若草稿放在独立卡上，这些激活每步都得跨卡搬运，延迟会抵消投机收益。vLLM、SGLang 的投机解码默认都与目标同卡运行。
- **TP2 切分，不是每卡一份完整副本**：SGLang 中 DFlash2 的线性层（`QKVParallelLinear` / `ColumnParallelLinear` / `RowParallelLinear`）直接复用目标模型的 TP 通信组，按注意力头切分（要求头数能被 tp_size 整除）。3.8 GB 的 checkpoint 在每张卡上实际占约 2.1 GB，与启动日志一致。candidate selector 每步做一次 all-gather 合并两卡候选后取最优。
- 独立卡方案（decoupled / disaggregated speculative decoding）适用于不依赖目标 hidden states 的独立小模型草稿，靠流水线重叠 draft 与 verify，不适用于本场景：DFlash2 架构上没有 embedding 和 lm_head，本身就不是独立模型，无法单独运行。

**选型结论**：追求极限单流速度且 4 卡可用 → 阶段 2（FP8 TP4 MTP，109 tok/s）；只用 2 卡、以单流/低并发为主 → 阶段 4（当前配置）；高并发（≥8 路）场景下阶段 4 反而不如阶段 3 的 MTP。

---

## 二、对比测试

测试方法：流式 OpenAI chat completions，temperature=0，`ignore_eos`，decode 速率不含 TTFT。单流三组 max_tokens=256，并发两组每路 128 token、thinking 关闭。同一脚本、同一组 prompt 跑完四种配置。

### 五组吞吐对比（tok/s）

| 配置 | GPU | Decode（短prompt） | Think-on | Long-prefill | Conc4 聚合 | Conc8 聚合 |
| --- | --- | --- | --- | --- | --- | --- |
| BF16 + MTP | 0–3, TP4 | 73.4 | 63.8 | 75.5 | 176.7 | 327.1 |
| FP8 + MTP | 0–3, TP4 | **109.1** | **95.6** | **112.7** | **224.3** | **373.2** |
| FP8 + MTP | 2–3, TP2 | 76.4 | 70.9 | 77.0 | 202.7 | 356.4 |
| FP8 + DFlash2（当前） | 2–3, TP2 | 96.1 | 88.1 | 90.4 | 220.3 | 227.5 |

### 延迟与容量

| 配置 | TTFT（短prompt） | KV 池容量（token） | 每卡权重占用 |
| --- | --- | --- | --- |
| BF16 + MTP, TP4 | 101 ms | 691,584 | ~13 GB |
| FP8 + MTP, TP4 | 89 ms | 897,088 | ~7.6 GB |
| FP8 + MTP, TP2 | 94 ms | 315,136 | ~15 GB |
| FP8 + DFlash2, TP2 | 92 ms | 245,696 | ~15 GB + 2.1 GB 草稿 |

注：Long-prefill（约 1,655 token 输入）首次 TTFT 含冷缓存约 0.6–1.2 s，前缀缓存命中后约 100–150 ms。

### 同卡（GPU 2+3, TP2）MTP vs DFlash2 逐项

| 指标 | MTP | DFlash2 | 变化 |
| --- | --- | --- | --- |
| Decode | 76.4 | 96.1 | **+26%** |
| Think-on | 70.9 | 88.1 | +24% |
| Long-prefill decode | 77.0 | 90.4 | +17% |
| Conc4 聚合 | 202.7 | 220.3 | +9% |
| Conc8 聚合 | 356.4 | 227.5 | **−36%** |
| KV 池 | 315,136 | 245,696 | −22% |

DFlash2 单流优势来自更高的平均接受长度（约 3.3–3.6 token/步 vs MTP 约 2.5–3.2）且草稿开销低；并发劣势来自 draft+verify 双阶段开销随 batch 增大、以及 verify CUDA graph 仅覆盖 bs=1–7。

---

## 三、复现配置

最终配置（FP8 TP2 + DFlash2）的关键启动参数，模型分别挂载为 `/models`（FP8 目标模型）和 `/draft`（[z-lab/Qwen3.8-27B-DFlash2](https://huggingface.co/z-lab/Qwen3.8-27B-DFlash2)）：

```bash
sglang serve \
  --model-path=/models \
  --trust-remote-code \
  --tp-size=2 \
  --attention-backend=flashinfer \
  --mem-fraction-static=0.80 \
  --mamba-full-memory-ratio=0.80 \
  --mamba-radix-cache-strategy=extra_buffer \
  --page-size=64 \
  --chunked-prefill-size=2048 \
  --context-length=262144 \
  --reasoning-parser=qwen3 \
  --tool-call-parser=qwen3_coder \
  --speculative-algorithm=DFLASH \
  --speculative-draft-model-path=/draft \
  --speculative-num-draft-tokens=8 \
  --speculative-draft-model-quantization=unquant \
  --speculative-draft-attention-backend=flashinfer
```

换回 MTP 只需删掉 5 个 `--speculative-*` 参数、换成 EAGLE 配置（steps=3, topk=1, draft tokens=4）。两卡合计显存占用约 76 GB（含 24.6 万 token 的 KV 池）。DFlash2 支持在 SGLang PR 35371 合入后的版本可用；更早的镜像需要自行从合入 commit 打 overlay 包（见下一节第 5 条）。

---

## 四、实施要点与坑

1. **Ampere 上 FP8 不是"原生 FP8 推理"**：走 Marlin W8A16，收益全部来自权重带宽，不要期待激活也变 FP8。
2. **DFlash2 与 MTP 互斥**：`-speculative-algorithm=DFLASH` 替换 EAGLE/MTP，不能叠加。
3. **草稿模型必须与目标同卡，且随目标 TP 切分**：DFlash2 依赖目标模型中间层 hidden states，独立放到空闲 GPU 0 的方案不成立（解耦投机 `decoupled-spec-role` 在该分支只有 IPC 桩，未接 DFLASH）。详见"草稿模型的放置与并行方式"一节。
4. **不要用改 `config.json` 架构名的方式把 DFlash2 灌进 DFlash v1**：v1 的 load_weights 会静默丢弃 `attention_conv` / `mlp_conv` / `candidate_selector` 权重，而主干是带 conv 训练的，结果会错。
5. **本地 overlay 的兼容修补**（qwen38 分支 vs main）：`dflash_worker_v2.py` 中 `get_schedule().page_size` 改为 `server_args.page_size`、去掉 `compute_spec_logprobs` 调用（该分支签名不同，logprobs 路径不用即可）；`dflash_utils.py` 中 `sample_simulated_acc_len` 需从 `_sample_simulated_acc_len` 别名导入。
6. FP8 checkpoint 的 `lm_head` 必须保持 BF16 稠密（在 `modules_to_not_convert` 中），DFlash2 的 candidate selector 依赖它。
