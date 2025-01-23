+++
author = "FlintyLemming"
title = "Hugo 搭配 Nginx 反代使用"
slug = "d1d694eeae164f16ae2b727fcd679eed"
date = "2022-09-24"
description = "主要就是在启动时加几个参数"
categories = ["MineService"]
tags = ["Blog", "Hugo"]
image = "https://image.mitsea.com/blog/posts/2022/09/Hugo%20%E6%90%AD%E9%85%8D%20Nginx%20%E5%8F%8D%E4%BB%A3%E4%BD%BF%E7%94%A8/title.avif"
+++

最近把博客迁移到了 Hugo，Hugo 并不算是个纯静态的博客系统。一般来说，是作为一个 go 服务放在后台运行的。通过 `hugo server` 命令可以启动为服务端，默认端口为 1313。但如果直接通过 Nginx 的 proxy_pass 反代为公网服务会遇到一些问题，应该加上一些参数启动。我的启动命令如下。

```bash
hugo server --appendPort=false --baseURL="https://blog.mitsea.com" --liveReloadPort 443
```

## appendPort

这里需要设置为 false，这样你的 baseURL 就不会变成 <域名>:1313。如果不设置为 false，你在使用搜索等功能的时候，浏览器还是会向 1313 端口发送请求。

## baseURL

这个没啥好说的，肯定要设置。

## liveReloadPort

这个是 Hugo 用来实时更新网页内容的 WebSocket 端口。除了要设置 Nginx 支持 WebSocket，这里还要手动指定下 wss 访问的端口为与 https 相同的 443。

更多命令与参数可以查看 Hugo 官方文档。

启动好后，再用 Nginx 的 proxy_pass 去反代 Hugo 的 127.0.0.1:1313 就行。

> Photo by [op23](https://unsplash.com/@op23?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
