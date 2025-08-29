+++
author = "FlintyLemming"
title = "Claude Code 搭配其他模型配置使用"
slug = "mvqBCMNbFMobQZtZ8k734q"
date = "2025-07-30"
description = "感谢开源模型"
categories = ["LifeTec"]
tags = ["Claude Code", "AI"]
image = "https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2025/07/Claude%20Code%20%E6%90%AD%E9%85%8D%E5%85%B6%E4%BB%96%E6%A8%A1%E5%9E%8B%E9%85%8D%E7%BD%AE%E4%BD%BF%E7%94%A8/norbert-kowalczyk-UAsQXiuTzSg-unsplash.avif"
+++

## 前提

Claude Code 要求模型需要有非常高的 Tool Calling 能力，并不是所有模型都可以很好的搭配 Claude Code 使用。除了国外闭源模型，我试下来 Qwen3 Coder 480B A35B Instruct 和 GLM 4.5 效果还不错，Kimi K2 据说效果也挺好。其他模型比如 Qwen3 系列的普通模型、DeepSeek 系列模型都不太能用。

其实你看模型介绍里关于 Tool Calling 的测试就行，它要是宣称自己 Tool Calling 好，那一般都没问题

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2025/07/Claude%20Code%20%E6%90%AD%E9%85%8D%E5%85%B6%E4%BB%96%E6%A8%A1%E5%9E%8B%E9%85%8D%E7%BD%AE%E4%BD%BF%E7%94%A8/image__LOIm_xsg-.avif)

图源：[GLM 4.5 技术文档](https://z.ai/blog/glm-4.5 "GLM 4.5 技术文档")

## 配置步骤

配置步骤默认你有良好的网络环境，安装过程已在 macOS 和 Windows 下验证过。Windows 需要允许 Powershell 执行第三方脚本和安装 Git，碰到的时候自己查一下都可以解决。

1. 安装 Node.js，用你熟悉的任何方法安装都可以。Windows 的话，可以直接装，也可以在 wsl 里用都行，安装完后通过执行下面两个命令检查是否安装成功。
   ```bash 
   node --version
   npm --version
   ```

2. 安装 Claude 本体、Claude Code Router 和 Claude Code Config
   ```javascript 
   npm install -g @anthropic-ai/claude-code
   npm install -g @musistudio/claude-code-router
   npm install -g @dashscope-js/claude-code-config

   ```

3. 执行 ccr-dashscope，会有一个引导你配置 Qwen 模型的过程，如果你用的就是官方的 Qwen 模型可以直接配，用别的模型就随便选择地区和token，后面会去手动改配置文件

   ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2025/07/Claude%20Code%20%E6%90%AD%E9%85%8D%E5%85%B6%E4%BB%96%E6%A8%A1%E5%9E%8B%E9%85%8D%E7%BD%AE%E4%BD%BF%E7%94%A8/image_1ZiyBG5fgs.avif)

   配完后，他会显示你的配置文件的目录，一般就在你的 home 目录下的 `.claude-code-router/config.json`，后面要改的就是这个文件
4. 打开配置文件，这边就比较清晰了，把模型地址、API、名字换一下就行。Dashscope 是阿里自己的东西，可以不动它。我用的 GLM 4.5，看一下我前后修改的位置

   ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2025/07/Claude%20Code%20%E6%90%AD%E9%85%8D%E5%85%B6%E4%BB%96%E6%A8%A1%E5%9E%8B%E9%85%8D%E7%BD%AE%E4%BD%BF%E7%94%A8/image_CkXgbqD1GG.avif)

   配置好后保存文件
5. 进入到你的项目文件夹，执行 `ccr code` 启动就可以使用了

## 其他问题

安装 Claude Code 后，进入 vscode 会自动安装插件，但是可以看到他这里还是使用的 claude 命令启动，这样启动是不会应用我们的配置的

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2025/07/Claude%20Code%20%E6%90%AD%E9%85%8D%E5%85%B6%E4%BB%96%E6%A8%A1%E5%9E%8B%E9%85%8D%E7%BD%AE%E4%BD%BF%E7%94%A8/image_lxB_GsP556.avif)

这里需要按 Ctrl + C，然后他会 fallback 到普通控制台，手动重新执行下 `ccr code` 即可，可以看到也是可以正常连接到 IDE 联动的

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2025/07/Claude%20Code%20%E6%90%AD%E9%85%8D%E5%85%B6%E4%BB%96%E6%A8%A1%E5%9E%8B%E9%85%8D%E7%BD%AE%E4%BD%BF%E7%94%A8/image_Pw2nQdZ4Oi.avif)

> Photo by [Norbert Kowalczyk](https://unsplash.com/@norbertkowalczyk?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/abstract-blue-waves-cascade-against-a-black-background-UAsQXiuTzSg?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)
      