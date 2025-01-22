+++
author = "FlintyLemming"
title = "Ghost Blog 迁移"
slug = "bc2a89d7b0e9444b9f4ce7bd94ef7338"
date = "2021-03-25"
description = ""
categories = ["Linux", "MineService"]
tags = ["Blog"]
image = "https://blog-img.mitsea.com/images/blog/posts/2019/10/Ghost%20Blog%20%E8%BF%81%E7%A7%BB/title.avif"
+++

## 获取原始站点数据

这里原来想复杂了，本打算迁移 MySQL 中 Ghost 对应的 Schema，但后来发现其实没必要

![](https://blog-img.mitsea.com/images/blog/posts/2019/10/Ghost%20Blog%20%E8%BF%81%E7%A7%BB/1.avif)

下面就以 Ghost 自带的功能介绍数据获取

### 获取文本数据

1. 打开 Ghost 的后台，左边找到 Labs，打开

    ![](https://blog-img.mitsea.com/images/blog/posts/2019/10/Ghost%20Blog%20%E8%BF%81%E7%A7%BB/2.avif)

2. 这里有个 Export your content，点击右侧的 Export，导出一个 json 文件，实际上这里已经包含了所有文本数据

    ![](https://blog-img.mitsea.com/images/blog/posts/2019/10/Ghost%20Blog%20%E8%BF%81%E7%A7%BB/3.avif)

### 获取媒体数据

除了文本之外，还需要获取两个重要内容，图片和主题（如果用了第三方）

1. 通过 sftp 连接到服务器

    ![](https://blog-img.mitsea.com/images/blog/posts/2019/10/Ghost%20Blog%20%E8%BF%81%E7%A7%BB/4.avif)

2. 定位到 Ghost 目录，官方默认的目录是 /var/www/ghost，然后打开里面的 content 目录

    ![](https://blog-img.mitsea.com/images/blog/posts/2019/10/Ghost%20Blog%20%E8%BF%81%E7%A7%BB/5.avif)

3. 需要里面的 images 和 themes，全部拷贝到本地

    ![](https://blog-img.mitsea.com/images/blog/posts/2019/10/Ghost%20Blog%20%E8%BF%81%E7%A7%BB/6.avif)

## 恢复数据

1. 在新站的文件目录下，恢复刚才 content 目录下的两个文件夹里的内容

    ![](https://blog-img.mitsea.com/images/blog/posts/2019/10/Ghost%20Blog%20%E8%BF%81%E7%A7%BB/7.avif)

2. 在新站的 Labs 里，导入刚才生成的 json 文件

    ![](https://blog-img.mitsea.com/images/blog/posts/2019/10/Ghost%20Blog%20%E8%BF%81%E7%A7%BB/8.avif)

3. 导入后刷新页面，即可看到文章和图片都已经恢复

    ![](https://blog-img.mitsea.com/images/blog/posts/2019/10/Ghost%20Blog%20%E8%BF%81%E7%A7%BB/9.avif)