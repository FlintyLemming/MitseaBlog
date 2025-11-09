+++
author = "FlintyLemming"
title = "Repomix 的应用实践"
slug = "8tLSfeFuEXoiZgTNPTfyaC"
date = "2024-11-11"
description = "LLM 应用实践之一"
categories = ["LifeTec"]
tags = ["LLM"]
image = "https://assets.mitsea.cn/blog/posts/2024/11/Repomix%20%E7%9A%84%E5%BA%94%E7%94%A8%E5%AE%9E%E8%B7%B5/susan-wilkinson-IIc73xHTRTg-unsplash.avif"
+++

[Repomix](https://github.com/yamadashy/repomix) 是一个将代码打包为一整个文本文件的工具，他非常适合在 AI Chat 上下文中需要引用代码的场景。下面我展示一个应用案例，用来应对使用的第三方包在大版本更新后使用方式上发生变化的应对。

我在某 iOS App 开发中使用了一个[第三方图片选择器](https://github.com/longitachi/ZLPhotoBrowser)，由于 iOS 18 的权限管理发生变化，我必须更新这个第三方包为最新版才可以正常使用。但是我先前用的版本非常老，导致直接更新软件包后，部分代码需要修改。

![](https://assets.mitsea.cn/blog/posts/2024/11/Repomix%20%E7%9A%84%E5%BA%94%E7%94%A8%E5%AE%9E%E8%B7%B5/CleanShot%202024-11-11%20at%2014.17.36%402x_xauF-Ktxli.avif)

可以看到，诸如 `Value of type 'ZLPhotoConfiguration' has no member 'editImageClipRatios'` 这样的错误显然就是由于更新包后，用法发生了变化，需要去查最新的文档，甚至是直接看源码。

这显然是非常浪费时间的，我们可以用 LLM 帮助我们修改代码。但是考虑这是一个冷门的软件包，LLM 不会有详细的信息，如果你直接去描述问题让 LLM 帮你去改，那一定会出现幻觉，LLM 最后帮你改出来的代码还是错的。

![](https://assets.mitsea.cn/blog/posts/2024/11/Repomix%20%E7%9A%84%E5%BA%94%E7%94%A8%E5%AE%9E%E8%B7%B5/CleanShot%202024-11-11%20at%2014.46.48%402x_LuCJJVonIi.avif)

比如这里 Claude 3.5 说 `clipRatios` 是直接设置在 `ZLPhotoConfiguration` 上的，实际上这还是错的，新版是设置在 `ZLPhotoConfiguration` 的 `editImageConfiguration` 里的。

这个时候就需要上传参考代码给 LLM 让他根据正确的代码去预测代码，这样就不容易出现幻觉。此时就需要 Repomix 了，这里使用 Repomix 将这个包的一个 Demo 文件夹打包成一个 txt 文件。

![](https://assets.mitsea.cn/blog/posts/2024/11/Repomix%20%E7%9A%84%E5%BA%94%E7%94%A8%E5%AE%9E%E8%B7%B5/CleanShot%202024-11-11%20at%2014.28.04%402x_xIHvuKjWzr.avif)

可以看到它连提示词和代码结构都帮你写好了。

![](https://assets.mitsea.cn/blog/posts/2024/11/Repomix%20%E7%9A%84%E5%BA%94%E7%94%A8%E5%AE%9E%E8%B7%B5/CleanShot%202024-11-11%20at%2014.45.40%402x_P3TpUEmS-s.avif)

这个时候你再去问 AI，他就知道在新版本里改如何设置编辑时候的属性了。

![](https://assets.mitsea.cn/blog/posts/2024/11/Repomix%20%E7%9A%84%E5%BA%94%E7%94%A8%E5%AE%9E%E8%B7%B5/CleanShot%202024-11-11%20at%2014.50.35%402x_xwcoLr0wLC.avif)

修改后的代码就是符合新版本的用法了，并且也保留了原来的功能。

> Photo by [Susan Wilkinson](https://unsplash.com/@susan_wilkinson?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/a-close-up-of-a-white-and-yellow-object-on-a-black-background-IIc73xHTRTg?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)
      
