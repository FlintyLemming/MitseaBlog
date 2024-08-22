+++
author = "FlintyLemming"
title = "【归档】iOS 修改 崩坏3 狂野飙车9 的帧率上限"
slug = "269e798744774d5f865260e6654d80cc"
date = "2020-06-01"
description = ""
categories = ["Apple", "Game"]
tags = ["iOS", "崩坏3", "狂野飙车9"]
image = "https://hf-public-source.mitsea.com:8840/images/blog/posts/2020/06/iOS%20%E4%BF%AE%E6%94%B9%20%E5%B4%A9%E5%9D%8F3%20%E7%8B%82%E9%87%8E%E9%A3%99%E8%BD%A69%20%E7%9A%84%E5%B8%A7%E7%8E%87%E4%B8%8A%E9%99%90/title.avif"
+++

## 前排提示

1. 修改工具要钱。
2. 修改是一次性的，游戏被杀掉后，再打开游戏需要重新修改。
3. 本质为修改内存数值，这与绝大部分外挂的本质操作相同，封号自负。

## 具体步骤

1. 在 iGameGuardian 的[官网](http://igg-server.herokuapp.com)购买、安装 iGameGuardian

2. 打开崩坏3，把帧率改成60fps

    ![](https://hf-public-source.mitsea.com:8840/images/blog/posts/2020/06/iOS%20%E4%BF%AE%E6%94%B9%20%E5%B4%A9%E5%9D%8F3%20%E7%8B%82%E9%87%8E%E9%A3%99%E8%BD%A69%20%E7%9A%84%E5%B8%A7%E7%8E%87%E4%B8%8A%E9%99%90/1.avif)

3. 打开 iGG，在“目标”里选择 bh3

    ![](https://hf-public-source.mitsea.com:8840/images/blog/posts/2020/06/iOS%20%E4%BF%AE%E6%94%B9%20%E5%B4%A9%E5%9D%8F3%20%E7%8B%82%E9%87%8E%E9%A3%99%E8%BD%A69%20%E7%9A%84%E5%B8%A7%E7%8E%87%E4%B8%8A%E9%99%90/2.avif)

4. 在“搜索”里点击“数值搜索”，搜索60，可以看到搜索很多很多值为60的项目。

5. 返回游戏，把帧率改成30。

    ![](https://hf-public-source.mitsea.com:8840/images/blog/posts/2020/06/iOS%20%E4%BF%AE%E6%94%B9%20%E5%B4%A9%E5%9D%8F3%20%E7%8B%82%E9%87%8E%E9%A3%99%E8%BD%A69%20%E7%9A%84%E5%B8%A7%E7%8E%87%E4%B8%8A%E9%99%90/3.avif)

6. 返回iGG，打开左上角的开关，点击“差值搜索”，搜索`-30`，即搜索60-30=30的数值

    ![](https://hf-public-source.mitsea.com:8840/images/blog/posts/2020/06/iOS%20%E4%BF%AE%E6%94%B9%20%E5%B4%A9%E5%9D%8F3%20%E7%8B%82%E9%87%8E%E9%A3%99%E8%BD%A69%20%E7%9A%84%E5%B8%A7%E7%8E%87%E4%B8%8A%E9%99%90/4.avif)

7. 此时可以看到之前数值为60的项目中，只有3个值现在是30

    ![](https://hf-public-source.mitsea.com:8840/images/blog/posts/2020/06/iOS%20%E4%BF%AE%E6%94%B9%20%E5%B4%A9%E5%9D%8F3%20%E7%8B%82%E9%87%8E%E9%A3%99%E8%BD%A69%20%E7%9A%84%E5%B8%A7%E7%8E%87%E4%B8%8A%E9%99%90/5.avif)

8. 其实只有一两个值和FPS上限相关，可以重复之前的操作，改成60再搜一次，这里就不继续了。长按搜到的这三个值，保存起来。

    ![](https://hf-public-source.mitsea.com:8840/images/blog/posts/2020/06/iOS%20%E4%BF%AE%E6%94%B9%20%E5%B4%A9%E5%9D%8F3%20%E7%8B%82%E9%87%8E%E9%A3%99%E8%BD%A69%20%E7%9A%84%E5%B8%A7%E7%8E%87%E4%B8%8A%E9%99%90/6.avif)

9. 在“记录”中看到保存的三个值，点击“全改”，改成120，然后打开后面的开关，锁定数值。修改完毕。

    ![](https://hf-public-source.mitsea.com:8840/images/blog/posts/2020/06/iOS%20%E4%BF%AE%E6%94%B9%20%E5%B4%A9%E5%9D%8F3%20%E7%8B%82%E9%87%8E%E9%A3%99%E8%BD%A69%20%E7%9A%84%E5%B8%A7%E7%8E%87%E4%B8%8A%E9%99%90/7.avif)

## 证明有效

1. 以崩坏3为例，修改完后返回设置，修改帧率为60，保存后点击主页面时，仍然会提示你保存，说明已经无法修改与帧率上限有关的内存数值，也就是第9步锁死的那些数值。

2. Asphalt 9同理，修改完后，即使设置里修改为60fps，返回，再点开设置，发现又变成30fps。
