+++
author = "FlintyLemming"
title = "【归档】500M 宽带当前是否有升级必要"
date = "2019-10-22"
description = ""
categories = ["Network"]
tags = ["宽带"]
image = "https://img.mitsea.com/blog/posts/2019/10/500M%20%E5%AE%BD%E5%B8%A6%E5%BD%93%E5%89%8D%E6%98%AF%E5%90%A6%E6%9C%89%E5%8D%87%E7%BA%A7%E5%BF%85%E8%A6%81/title.jpg?x-oss-process=style/ImageCompress"
+++

到南京之后，一直用的 500M 宽带，用了大约四个多月之后，简单分析下是否值得升级到 500M 宽带。总体来说，是不建议升级的，所以先说优点，再说缺点。

## 优点

### 下载速度的单纯提高

先来测个速吧，首先是 Speedtest，这个是写这篇文章的时候现测的，可以看到今晚貌似用的人不多，带宽给的比较足

![](https://img.mitsea.com/blog/posts/2019/10/500M%20%E5%AE%BD%E5%B8%A6%E5%BD%93%E5%89%8D%E6%98%AF%E5%90%A6%E6%9C%89%E5%8D%87%E7%BA%A7%E5%BF%85%E8%A6%81/1.png?x-oss-process=style/ImageCompress)

实际上这也是 500M 宽带本身的一个优势，就是使用的人并不会太多，而提供 500M 宽带的小区单元给的带宽往往都不会太小，所以经常能有个很高的速度。

不过偶尔也有稍微差一点点的时候，比如这个 Netflix 网速测试，不过这个可能跟代理当时的线路有一定关系

![](https://img.mitsea.com/blog/posts/2019/10/500M%20%E5%AE%BD%E5%B8%A6%E5%BD%93%E5%89%8D%E6%98%AF%E5%90%A6%E6%9C%89%E5%8D%87%E7%BA%A7%E5%BF%85%E8%A6%81/2.png?x-oss-process=style/ImageCompress)

### 节省时间

因为平时上班回家后其实使用电脑的时间并不算多，如果需要下载什么文件，尤其是大文件，都会节省大量晚上的宝贵时间，不需要耗费很多时间在等待上。这对整个使用时间和逻辑的安排都会产生非常大的影响。

### 减少QoS的精力

以前使用 100M 小水管的时候，即便不是单纯下载，也经常会因为多个设备使用网络造成带宽堵塞。换上 500M 的大水管后，基本无需操心网络的 QoS，即便有机器在下载，也很少影响其他设备的普通网络需求（至于为什么会在缺点中解释）。

## 缺点

### 网络质量并不会有提升

换上更大带宽的网络，只会单纯提高速度，并不会提高网络质量。这里网络质量包含很多，我举几个例子。

#### 原先访问缓慢的速度还是缓慢

如果你觉得以前访问某些互联网服务觉得缓慢，比如 Windows 更新下载慢，OneDrive 同步慢，AppStore 抽风，GitHub 拉代码慢，等等。这些都不会因为你更换了更快的宽带有任何改变。

#### 原来网页不能秒开现在可以了？并不

实际上从 HTTP 2.0 开始，就可以并发请求了（1.1 的 pipeline 忽略），即便你用的 5M、10M 小水管，打开网页速度也并不会太慢，顶多是网页打开后，里面的图片等元素还需要时间。但是想要一个更快的网页打开速度，并不能简单通过增大网络带宽解决，你需要向其他方面检查。比如你的 DNS 查询是不是并发等等。

#### 很多下载场景并不能有效利用带宽

记得 Bilibili up主 老师好我叫何同学那个关于 5G 的视频里提到这么一个场景，大致意思就是说，当游戏如果蹦出一个几百M的更新，4G 可能需要等很久，打搅了性质，5G 并不会。但最后他也没给出 5G 下，王者荣耀的游戏内更新到底有多快。

![](https://img.mitsea.com/blog/posts/2019/10/500M%20%E5%AE%BD%E5%B8%A6%E5%BD%93%E5%89%8D%E6%98%AF%E5%90%A6%E6%9C%89%E5%8D%87%E7%BA%A7%E5%BF%85%E8%A6%81/3.png?x-oss-process=style/ImageCompress)

那么？到底有多快呢？500M 的宽带告诉你，并没有多快。王者荣耀的应用内更新我从没见过比 10M/s 高很多的速度，而 500M 的宽带，就算不按正常单位换算，你至少也要有 50M/s 的速度。以我的 iPhone 7 为例，速度大约是 5M/s，这也就是个 4G 网络的速度。

![](https://img.mitsea.com/blog/posts/2019/10/500M%20%E5%AE%BD%E5%B8%A6%E5%BD%93%E5%89%8D%E6%98%AF%E5%90%A6%E6%9C%89%E5%8D%87%E7%BA%A7%E5%BF%85%E8%A6%81/4.png?x-oss-process=style/ImageCompress)

为什么会这样呢，主要是你的网络准备好了足够的带宽，但是服务提供商并没有准备好。无论是直接从原始服务器还是从 CDN 下载，他们给你所能提供的最大速度甚至只有 200M 甚至 100M 带宽的水平。

实际上下载文件也是，很多服务器所能提供的最大速度也不高，而有的则是因为线路问题，到你电脑上速度就只有一点了，很多时候需要依赖迅雷这样的 P2P 下载器才能跑满带宽。

![](https://img.mitsea.com/blog/posts/2019/10/500M%20%E5%AE%BD%E5%B8%A6%E5%BD%93%E5%89%8D%E6%98%AF%E5%90%A6%E6%9C%89%E5%8D%87%E7%BA%A7%E5%BF%85%E8%A6%81/5.png?x-oss-process=style/ImageCompress)

是否使用下载器的速度对比

![](https://img.mitsea.com/blog/posts/2019/10/500M%20%E5%AE%BD%E5%B8%A6%E5%BD%93%E5%89%8D%E6%98%AF%E5%90%A6%E6%9C%89%E5%8D%87%E7%BA%A7%E5%BF%85%E8%A6%81/6.png?x-oss-process=style/ImageCompress)

那如果你是个不喜欢使用毒瘤软件的人，就会很麻烦，因为这种你用其他下载器，拉很多线程也无法解决。

### 网络设备的大量开销

#### 路由器

你不仅需要一个有线能够保证跑满 500M 的路由器，还要保证他的 Wi-Fi 也能跑满 500M，一个 Wi-Fi 能跑满 500M 的路由器，基本就告别百元范畴，2、300朝上走了，甚至更贵。而且只有 5G 频段能跑满这个速度，但 5G 频段的信号衰减大，如果是家庭方案，需要非常多的节点，整个时间和金钱成本就上去了。

不过如果你是和我一样在外地租一个房间住，那就只需要一个路由器，这里推荐一下我一直用的华为 WS5200 增强版，增强版已经停产，不太好买，四核版也可以。这个路由性能还可以，不仅能够提供足够的外网速度，内网也很不错，传输单个大文件可以达到 90-100M/s 的速度。

![](https://img.mitsea.com/blog/posts/2019/10/500M%20%E5%AE%BD%E5%B8%A6%E5%BD%93%E5%89%8D%E6%98%AF%E5%90%A6%E6%9C%89%E5%8D%87%E7%BA%A7%E5%BF%85%E8%A6%81/7.jpg?x-oss-process=style/ImageCompress)

#### 移动终端

要知道你较早的设备可能也无法跑到 500M 的速度，iPhone 的话，大约是 iPhone 6s 和之前的设备都不可以。Android 可以在 Wi-Fi 中查看链接详细信息，如果 传输链接速度 只有 433 Mbps，那就不可以，这些大多是老设备或者刻意被厂商阉割过天线。

![](https://img.mitsea.com/blog/posts/2019/10/500M%20%E5%AE%BD%E5%B8%A6%E5%BD%93%E5%89%8D%E6%98%AF%E5%90%A6%E6%9C%89%E5%8D%87%E7%BA%A7%E5%BF%85%E8%A6%81/8.png?x-oss-process=style/ImageCompress)

比如我手里的 MIX 1，便只能跑到这个速度。

![](https://img.mitsea.com/blog/posts/2019/10/500M%20%E5%AE%BD%E5%B8%A6%E5%BD%93%E5%89%8D%E6%98%AF%E5%90%A6%E6%9C%89%E5%8D%87%E7%BA%A7%E5%BF%85%E8%A6%81/9.png?x-oss-process=style/ImageCompress)

### 仍然不算快的上传

使用了 500M 的宽带并不能为你带来成比例的上行带宽的增加。按照工信部的建议，家庭宽带的上传应当为下行速度的 20%，但最大只能为 50M，所以有高上传速度需求的用户应该想想别的方法。

### 价格

我这边 500M 宽带月费299元，但目前有每月反100元的活动，等于一个月199，但活动结束后是只能299继续用，还是有别的活动就不得而知了。总的来说并不算便宜，每个月缴费时还是会稍稍肉疼一下。

## 总结

总结下来，升级 500M 宽带目前整体体验并不算太好，如果有人想你推介，一定要认真考虑清楚了。个人建议 200M 是一个比较均衡的选择。