+++
author = "FlintyLemming"
title = "上传歌曲到 Apple Music iCloud 资料库"
slug = "7b0a3b0bb7144692950a4377dfa91ab9"
date = "2019-10-09"
description = ""
categories = ["Apple"]
tags = ["Apple Music"]
image = "https://assets.mitsea.cn/blog/posts/2019/10/%E4%B8%8A%E4%BC%A0%E6%AD%8C%E6%9B%B2%E5%88%B0%20Apple%20Music%20iCloud%20%E8%B5%84%E6%96%99%E5%BA%93/title.avif"
+++

Apple Music iCloud 资料库类似一个音乐云盘，如果你用过网易云音乐或者 Google Play Music 应该熟悉这个概念。通过上传歌曲，可以很好补充平台缺失的歌曲，以后听歌也不需要反复切换应用。
对于 Apple Music iCloud 资料库，它提供独立于 iCloud 存储以外的10万首歌曲的配额，无需担心容量问题。
其实上传本身非常简单，本不用大费周章写一篇文章，但其中坑点较多，我会尽量反映我遇到的问题并给出对应。

## 获取歌曲
最主要的就是要注意你的音源格式，mp3等标签功能较为丰富的格式没有问题，而flac、wav这种原生不支持嵌入专辑封面的格式则需要处理。
此外，如果你使用的 macOS，尤其是 macOS 10.14 及之前的 iTunes，个人甚至不建议直接上传 mp3 格式的音源，可能会遇到上传后在别的设备听发现不完整的情况。
最适合上传的格式是alac或者aiff，当然alac封装的文件后缀并不是alac，是m4a，这里提一下，免得后面不认识。
如果你是直接下载的那比较好办，比如 BandCamp 这种都可以自己选择下载的格式，那你下载 alac 或者 aiff 的就行。
    ![](https://assets.mitsea.cn/blog/posts/2019/10/%E4%B8%8A%E4%BC%A0%E6%AD%8C%E6%9B%B2%E5%88%B0%20Apple%20Music%20iCloud%20%E8%B5%84%E6%96%99%E5%BA%93/1.avif)

## 处理歌曲
如果下载的格式只有 flac 那就转成 alac，如果是 wav 那就转成 aiff，如果是 mp3 那就随便。至于转换工具，macOS 有个非常不错的免费工具，点这个[链接](https://apps.apple.com/cn/app/music-convert-audio-converter/id1036029895?mt=12)跳转商店下载，Windows 我就不知道了。如果软件带标签编辑功能，你就可以在转换前把歌名、歌手、专辑封面什么的该填的填好，然后再转换。当然，也可以在 iTunes（或者 macOS 10.15 里的 音乐，后面仍然叫 iTunes）编辑，这个会在上传歌曲部分提及。

## 上传歌曲
上传非常简单，你只需要保证 iTunes 已经登陆你的 Apple Music 账号，并且开启了资料库同步，也就是你在 iTunes 能看到自己的歌单就行。然后只需要把文件拖进去就可以了。
    ![](https://assets.mitsea.cn/blog/posts/2019/10/%E4%B8%8A%E4%BC%A0%E6%AD%8C%E6%9B%B2%E5%88%B0%20Apple%20Music%20iCloud%20%E8%B5%84%E6%96%99%E5%BA%93/2.avif)

拖进去后，状态会显示成“等待上传”，云标志会显示成虚线轮廓。
    ![](https://assets.mitsea.cn/blog/posts/2019/10/%E4%B8%8A%E4%BC%A0%E6%AD%8C%E6%9B%B2%E5%88%B0%20Apple%20Music%20iCloud%20%E8%B5%84%E6%96%99%E5%BA%93/3.avif)

如果你看不到“云端状态”，右键分类栏，可以调出来，我是建议打开的。
    ![](https://assets.mitsea.cn/blog/posts/2019/10/%E4%B8%8A%E4%BC%A0%E6%AD%8C%E6%9B%B2%E5%88%B0%20Apple%20Music%20iCloud%20%E8%B5%84%E6%96%99%E5%BA%93/4.avif)

它不会立即上传，会等待你继续上传其他歌曲或者编辑完资料后不操作一段时间后才会上传。此时你可以编辑资料，右键歌曲，显示简介。
    ![](https://assets.mitsea.cn/blog/posts/2019/10/%E4%B8%8A%E4%BC%A0%E6%AD%8C%E6%9B%B2%E5%88%B0%20Apple%20Music%20iCloud%20%E8%B5%84%E6%96%99%E5%BA%93/5.avif)

在这边你可以编辑信息，在插图那里也可以修改封面，只需要把图片拖进去就行。关于专辑封面，你可以尝试在这里找找有没有自己需要的：http://coverbox.henry-hu.com。
    ![](https://assets.mitsea.cn/blog/posts/2019/10/%E4%B8%8A%E4%BC%A0%E6%AD%8C%E6%9B%B2%E5%88%B0%20Apple%20Music%20iCloud%20%E8%B5%84%E6%96%99%E5%BA%93/6.avif)

上传完后有两个可能，如果你上传的歌曲 Apple Music 没有，那么就会显示“已上传”。如果 Apple Music 有（可能是别的区），那可能会显示“已匹配”。这两个带来的结果是有区别的，后面我会讲。

## 下载和聆听
在电脑上上传歌曲后，其他设备就可以听了，与正常歌曲无异。但又有细微区别，首先是链接速度，由于Apple Music自己曲库的服务器和存储你歌曲的服务器不在一个地方，所以在中国大陆，聆听和下载你上传的歌曲可能会比较慢。尤其是 Android 端和网页版。但是正如我前面提到已上传和已匹配是不一样的，如果匹配上，其他设备下载会从Apple Music自己的曲库下载，速度好一些。

此外，自己上传的歌曲，在设备切换 AppStore 商店账号的时候是不会丢失的，而从曲库添加到资料库的则会全部丢失本地文件。

> Photo by [Hanny Naibaho](https://unsplash.com/@hannynaibaho?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/music?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
  
