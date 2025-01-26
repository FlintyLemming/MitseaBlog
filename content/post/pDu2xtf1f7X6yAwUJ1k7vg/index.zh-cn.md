+++
author = "FlintyLemming"
title = "拷打付费软件 —— MT Photos"
slug = "pDu2xtf1f7X6yAwUJ1k7vg"
date = "2024-12-12"
description = "第二款软件"
categories = ["HomeLab"]
tags = ["NAS"]
image = "https://hf-image.mitsea.com:8840/blog/posts/2024/12/%E6%8B%B7%E6%89%93%E4%BB%98%E8%B4%B9%E8%BD%AF%E4%BB%B6%20%E2%80%94%E2%80%94%20MT%20Photos/david-becker-mGx5-xt1uec-unsplash.avif"
+++

通过对比，个人觉得 MT Photos 撑不起现在这个价格，相比于 immich 没有明显优势，在部分体验上甚至有劣势，下面整理下我遇到的主要问题

1. 资源管理差

   人脸识别任务完成后两三个小时都不自动回收显存

   ![](https://hf-image.mitsea.com:8840/blog/posts/2024/12/%E6%8B%B7%E6%89%93%E4%BB%98%E8%B4%B9%E8%BD%AF%E4%BB%B6%20%E2%80%94%E2%80%94%20MT%20Photos/CleanShot%202024-12-12%20at%2014.48.28@2x_sULGJeWc0y.avif)

   原因非常的抽象，它的前端会定时调 API 的 /check

   ![](https://hf-image.mitsea.com:8840/blog/posts/2024/12/%E6%8B%B7%E6%89%93%E4%BB%98%E8%B4%B9%E8%BD%AF%E4%BB%B6%20%E2%80%94%E2%80%94%20MT%20Photos/742e29f3b2a91faf7822e4b2242bb391_kVFQv-yZaF.avif)

   然后他后端的重启逻辑是什么呢，只要有 API 调用，就一个小时内都不会重启

   ![](https://hf-image.mitsea.com:8840/blog/posts/2024/12/%E6%8B%B7%E6%89%93%E4%BB%98%E8%B4%B9%E8%BD%AF%E4%BB%B6%20%E2%80%94%E2%80%94%20MT%20Photos/CleanShot%202024-12-12%20at%2015.56.35@2x_4hdVCRuv2X.avif)

   所以他就永远都不会重启释放显存

2. 扫库效率低下

   它的缩略图、元信息提取、生成缩略图等任务有可能不是并行的，至少前端看不出来。导致初始化时扫库效率明显低于 immich（任务调度已经拉满CPU 64 线程）

   ![](https://hf-image.mitsea.com:8840/blog/posts/2024/12/%E6%8B%B7%E6%89%93%E4%BB%98%E8%B4%B9%E8%BD%AF%E4%BB%B6%20%E2%80%94%E2%80%94%20MT%20Photos/a11188addded8db82b5ad8cb0cfc0770_7sT82iU1OX.avif)

   immich 多个任务是并行的

   ![](https://hf-image.mitsea.com:8840/blog/posts/2024/12/%E6%8B%B7%E6%89%93%E4%BB%98%E8%B4%B9%E8%BD%AF%E4%BB%B6%20%E2%80%94%E2%80%94%20MT%20Photos/fd8cf3699c1cb047d494de787252d779_rZ6XiWbOcq.avif)

   ![](https://hf-image.mitsea.com:8840/blog/posts/2024/12/%E6%8B%B7%E6%89%93%E4%BB%98%E8%B4%B9%E8%BD%AF%E4%BB%B6%20%E2%80%94%E2%80%94%20MT%20Photos/0e20dec3012821d20afccdb395819e7f_gw9jj7je7x.avif)

3. CLIP 搜图模型效果不佳

   默认用的 Chinese-CLIP 而且不能更换，实测效果不如 immich 的 XLM-Roberta-Large-Vit-B-16Plus。比如搜索血压计，MT Photos 搜不出来

   ![](https://hf-image.mitsea.com:8840/blog/posts/2024/12/%E6%8B%B7%E6%89%93%E4%BB%98%E8%B4%B9%E8%BD%AF%E4%BB%B6%20%E2%80%94%E2%80%94%20MT%20Photos/ff8ecea0addf80e86509a43310817a7b_RtHxxStVTw.avif)

   ![](https://hf-image.mitsea.com:8840/blog/posts/2024/12/%E6%8B%B7%E6%89%93%E4%BB%98%E8%B4%B9%E8%BD%AF%E4%BB%B6%20%E2%80%94%E2%80%94%20MT%20Photos/10ef53de74add70cb4871c9e618b888b_uGCxT3eYtE.avif)

4. 文档质量一般

   首先他这个 docker compose 配置文件写的就不合理，两个依赖的 ai 容器根本就不需要开放端口，直接用 compose 内部网络就可以链接，即 `容器名:端口`

   ![](https://hf-image.mitsea.com:8840/blog/posts/2024/12/%E6%8B%B7%E6%89%93%E4%BB%98%E8%B4%B9%E8%BD%AF%E4%BB%B6%20%E2%80%94%E2%80%94%20MT%20Photos/CleanShot%202024-12-12%20at%2015.14.42@2x_By2hr_RJZ6.avif)

   然后他本来是基于 docker compose 提供的部署教程，这里又变成了 docker run，第一次用的用户可能就不知道这个 --gpu all 如果你改 docker compose 里面的镜像，还需要添加 GPU 设备才可以

   ![](https://hf-image.mitsea.com:8840/blog/posts/2024/12/%E6%8B%B7%E6%89%93%E4%BB%98%E8%B4%B9%E8%BD%AF%E4%BB%B6%20%E2%80%94%E2%80%94%20MT%20Photos/CleanShot%202024-12-12%20at%2015.34.05@2x_pTPBh3N4sn.avif)

   ![](https://hf-image.mitsea.com:8840/blog/posts/2024/12/%E6%8B%B7%E6%89%93%E4%BB%98%E8%B4%B9%E8%BD%AF%E4%BB%B6%20%E2%80%94%E2%80%94%20MT%20Photos/CleanShot%202024-12-12%20at%2015.20.12@2x_2ipqQyqTZl.avif)

   总而言之就是非常混乱，你不如直接给四个 docker compose 配置文件：不带 AI 的，带 onnx AI 的，带 cuda AI 的，带 arm AI 的就完事了。不要把面向客户的文档写成了自己看的笔记。

5. GPS API 服务需要用户自己配

   这个我觉得是最抽象的，immich 作为免费开源软件都默认提供 GPS API，而且国内照片位置也正常，但是 MT Photos 作为收费软件还需要自己去申请 API 配进去。

   [https://mtmt.tech/docs/start/gps\_api](https://mtmt.tech/docs/start/gps_api "https://mtmt.tech/docs/start/gps_api")

6. Android App 体验不好

   不支持 HDR 视频回看

   开了底部导航显示优化后查看原图还是用的全屏模式（Immersive Mode），需要上划底部才能呼出导航栏

   等等等等小问题

> Photo by [David Becker](https://unsplash.com/@beckerworks?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/a-mountain-covered-in-snow-under-a-cloudy-sky-mGx5-xt1uec?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)
      