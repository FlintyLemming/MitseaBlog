+++
author = "FlintyLemming"
title = "vLLM 部署 Qwen3 Coder 模型"
slug = "eLBq4yDXSuCAP1zP7dPYnq"
date = "2025-07-31"
description = "建议就用 256k 上下文，1M Prefill 太慢了"
categories = ["Coding"]
tags = ["Qwen", "AI"]
image = "https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2025/07/vLLM%20%E9%83%A8%E7%BD%B2%20Qwen3%20Coder%20%E6%A8%A1%E5%9E%8B/vadim-kaipov-WA2YYz0tIFY-unsplash.avif"
+++

## 文档参照

Qwen 官方已经更新了他们的文档，部署中遇到的问题在这里看[文档](https://qwen.readthedocs.io/zh-cn/latest/deployment/vllm.html)都能解决。只不过他没有给你现成的命令，需要你自己理解一下。（虽然我也不好说自己有没有理解对.jpg）

其次 vLLM 官方的[这个文档](https://docs.vllm.ai/projects/recipes/en/latest/Qwen/Qwen3-Coder-480B-A35B.html)不要看，方法是错的，我怀疑已经废弃但是没有删。

本教程使用 H200 GPU。

## 256k 上下文启动

按照以下命令启动即可

```bash 
vllm serve /mnt/extend/models/llm/Qwen/Qwen3-Coder-480B-A35B-Instruct-FP8 \
  --tensor-parallel-size 8 \
  --enable-expert-parallel \
  --enable-auto-tool-choice \
  --tool-call-parser qwen3_coder \
  --served-model-name Qwen3-Coder-480B-A35B-Instruct
```


启动后默认就是 256k 上下文

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2025/07/vLLM%20%E9%83%A8%E7%BD%B2%20Qwen3%20Coder%20%E6%A8%A1%E5%9E%8B/image_TvuskXp7i8.avif)

可以看到，每张卡剩余 61.31GB 显存，能开出来 2,073,680 tokens 的 KV Cache

## 1M 上下文启动

尝试直接按照下面的命令启动会报错

```bash 
vllm serve /mnt/extend/models/llm/Qwen/Qwen3-Coder-480B-A35B-Instruct-FP8 \
  --tensor-parallel-size 8 \
  --enable-expert-parallel \
  --max-model-len 1048576 \
  --enable-auto-tool-choice \
  --tool-call-parser qwen3_coder \
  --served-model-name Qwen3-Coder-480B-A35B-Instruct
```


报这个错

```python 
Value error, User-specified max_model_len (1048576) is greater than the derived max_model_len (max_position_embeddings=262144 or model_max_length=None in model's config.json). This may lead to incorrect model outputs or CUDA errors. To allow overriding this maximum, set the env var VLLM_ALLOW_LONG_MAX_MODEL_LEN=1 [type=value_error, input_value=ArgsKwargs((), {'model': ...attention_dtype': None}), input_type=ArgsKwargs]
```


然后我就尝试修改模型的 `config.json`

```json 
"max_position_embeddings": 1048576,
  ...
  "rope_scaling": {
    "type": "yarn",
    "factor": 4.0,
    "original_max_position_embeddings": 262144
  },
```


或者不改配置文件也行，直接把这个配置加到启动参数里

```bash 
VLLM_ALLOW_LONG_MAX_MODEL_LEN=1 vllm serve /mnt/extend/models/llm/Qwen/Qwen3-Coder-480B-A35B-Instruct-FP8 \
  --tensor-parallel-size 8 \
  --enable-expert-parallel \
  --max-model-len 1048576 \
  --rope-scaling '{"rope_type":"yarn","factor":4.0,"original_max_position_embeddings":262144}' \
  --enable-auto-tool-choice \
  --tool-call-parser qwen3_coder \
  --served-model-name Qwen3-Coder-480B-A35B-Instruct
```


这样就是 1M 上下文了

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2025/07/vLLM%20%E9%83%A8%E7%BD%B2%20Qwen3%20Coder%20%E6%A8%A1%E5%9E%8B/image_nv03Dz6S9a.avif)

> Photo by [vadim kaipov](https://unsplash.com/@vadimkaipov?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/abstracted-view-of-a-forest-with-blurred-lines-WA2YYz0tIFY?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)