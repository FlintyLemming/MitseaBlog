+++
author = "FlintyLemming"
title = "Windows Electron QQ 资源占用观察"
slug = "09605e45ed8141468a1d64ae81f7148e"
date = "2023-04-07"
description = "原本 8G 显存玩 2K 3A 的同学这下 QQ 都不敢挂后台了"
categories = ["LifeTec"]
tags = ["QQ", "内存", "Electron"]
image = "https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/04/Windows%20Electron%20QQ%20%E8%B5%84%E6%BA%90%E5%8D%A0%E7%94%A8%E8%A7%82%E5%AF%9F/malik-skydsgaard-rhqsKzvyyQo-unsplash.avif"
+++

## 说明

测试方法是通过对比打开和关闭程序前后的内存和显存数值，只看任务管理器某几个进程的内存占用之和是不准确的

## 具体测试

### 环境1 有独立显卡的台式机

32G DDR5 内存+3090 24G 显存

| 测试条件 | 内存占用 | 显存占用 |
| --- | --- | --- |
| QQ本体 | ~300M | ~400M |
| QQ本体+频道 | ~500M | ~700M |

### 环境2 没有显卡的服务器

128G 内存 远程桌面

| 测试条件 | 内存占用 | 显存占用 |
| --- | --- | --- |
| QQ本体 | ~600M | 0M |
| QQ本体+频道 | ~900M | 0M |

### 环境3 核显轻薄本

AMD Ryzen 5 5500U，核显分配 1G 内存

| 测试条件 | 内存占用 | 显存占用 |
| --- | --- | --- |
| QQ本体 | ~400M | ~300M |
| QQ本体+频道 | ~800M | ~600M |

注：显存在打开新频道对话时会达到1G，爆显存，建议显存分配2G

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/04/Windows%20Electron%20QQ%20%E8%B5%84%E6%BA%90%E5%8D%A0%E7%94%A8%E8%A7%82%E5%AF%9F/Untitled.avif)

## 其他观察

### 突发占用

当新打开一个 QQ 群或者频道聊天时，显存会有一个突发占用。比如这里两张图，第一张是刚打开一个聊天的时候，显存总占用 2.6G，静等一会，会掉到 2.3G。

如果你的显存没有这额外的 300M，我不确定会不会影响性能。

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/04/Windows%20Electron%20QQ%20%E8%B5%84%E6%BA%90%E5%8D%A0%E7%94%A8%E8%A7%82%E5%AF%9F/Untitled%201.avif)

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/04/Windows%20Electron%20QQ%20%E8%B5%84%E6%BA%90%E5%8D%A0%E7%94%A8%E8%A7%82%E5%AF%9F/Untitled%202.avif)

## 附件

### 环境1截图

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/04/Windows%20Electron%20QQ%20%E8%B5%84%E6%BA%90%E5%8D%A0%E7%94%A8%E8%A7%82%E5%AF%9F/Snipaste_2023-03-24_19-13-00.avif)

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/04/Windows%20Electron%20QQ%20%E8%B5%84%E6%BA%90%E5%8D%A0%E7%94%A8%E8%A7%82%E5%AF%9F/Snipaste_2023-03-24_19-14-25.avif)

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/04/Windows%20Electron%20QQ%20%E8%B5%84%E6%BA%90%E5%8D%A0%E7%94%A8%E8%A7%82%E5%AF%9F/Snipaste_2023-03-24_19-14-46.avif)

### 环境2截图

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/04/Windows%20Electron%20QQ%20%E8%B5%84%E6%BA%90%E5%8D%A0%E7%94%A8%E8%A7%82%E5%AF%9F/Untitled%203.avif)

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/04/Windows%20Electron%20QQ%20%E8%B5%84%E6%BA%90%E5%8D%A0%E7%94%A8%E8%A7%82%E5%AF%9F/Untitled%204.avif)

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/04/Windows%20Electron%20QQ%20%E8%B5%84%E6%BA%90%E5%8D%A0%E7%94%A8%E8%A7%82%E5%AF%9F/Untitled%205.avif)

### 环境3截图

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/04/Windows%20Electron%20QQ%20%E8%B5%84%E6%BA%90%E5%8D%A0%E7%94%A8%E8%A7%82%E5%AF%9F/Snipaste_2023-03-24_20-08-56.avif)

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/04/Windows%20Electron%20QQ%20%E8%B5%84%E6%BA%90%E5%8D%A0%E7%94%A8%E8%A7%82%E5%AF%9F/Snipaste_2023-03-24_20-08-30.avif)

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/04/Windows%20Electron%20QQ%20%E8%B5%84%E6%BA%90%E5%8D%A0%E7%94%A8%E8%A7%82%E5%AF%9F/Snipaste_2023-03-24_20-07-46.avif)

> Photo by [Malik Skydsgaard](https://unsplash.com/@malikskyds?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
  