+++
author = "FlintyLemming"
title = "华为昇腾 Ascend 910B 一览及对应整机"
slug = "1ab7bda595c58077852bc0ce54e6d1f3"
date = "2025-03-03"
description = "主要是没 fp8 单元，所以成本比较高"
categories = ["Consumer"]
tags = ["华为"]
image = "https://img.mitsea.com/blog/posts/2025/03/%E5%8D%8E%E4%B8%BA%E6%98%87%E8%85%BE%20Ascend%20910B%20%E4%B8%80%E8%A7%88%E5%8F%8A%E5%AF%B9%E5%BA%94%E6%95%B4%E6%9C%BA/a-chosen-soul-fwGtrR0ujbM-unsplash.avif"
+++

| NPU 型号 | FP16 算力 | 显存 | 对应华为整机 | 芯片代工厂 |
| --- | --- | --- | --- | --- |
| 昇腾 Ascend 910B4 | 280T | 32GB HBM2 | Atlas 800I A2 | 中芯国际 |
| 昇腾 Ascend 910B3 | 313T | 64GB HBM2 | Atlas 800T A2 | 中芯国际 |
| 昇腾 Ascend 910B2 | 376T | 64GB HBM2 |  | 台积电 |
| 昇腾 Ascend 910B1 | 414T | 64GB HBM2 |  | 台积电 |

由于没有 fp8 计算单元，若要部署完整精度的 Deepseek R1，则需要将近 2T 显存，需要至少 4 台 8 卡 32GB 版本的 910B

> Photo by [A Chosen Soul](https://unsplash.com/@a_chosensoul?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/a-computer-generated-image-of-a-womans-face-fwGtrR0ujbM?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)
      