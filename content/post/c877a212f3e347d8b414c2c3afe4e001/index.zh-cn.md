+++
author = "FlintyLemming"
title = "FreeBSD 更换中科大软件包源"
slug = "c877a212f3e347d8b414c2c3afe4e001"
date = "2023-04-04"
description = ""
categories = ["MineService"]
tags = ["FreeBSD"]
image = "https://img.mitsea.com/blog/posts/2023/04/FreeBSD%20%E6%9B%B4%E6%8D%A2%E4%B8%AD%E7%A7%91%E5%A4%A7%E8%BD%AF%E4%BB%B6%E5%8C%85%E6%BA%90/milad-fakurian-r1JDkQgwnC4-unsplash.jpg?x-oss-process=style/ImageCompress"
+++

1. 创建 /usr/local/etc/pkg/repos 文件夹

    ```bash
    mkdir -p /usr/local/etc/pkg/repos
    ```

2. 创建 /usr/local/etc/pkg/repos/FreeBSD.conf 文件

    ```bash
    ee /usr/local/etc/pkg/repos/FreeBSD.conf
    ```

    文件内容如下

    ```json
    FreeBSD: {
      url: "pkg+http://mirrors.ustc.edu.cn/freebsd-pkg/${ABI}/quarterly",
    }
    ```

    编辑完毕后，按一下 Esc，按一下 A，选择 Leave Editor

    ![](https://img.mitsea.com/blog/posts/2023/04/FreeBSD%20%E6%9B%B4%E6%8D%A2%E4%B8%AD%E7%A7%91%E5%A4%A7%E8%BD%AF%E4%BB%B6%E5%8C%85%E6%BA%90/Untitled.png?x-oss-process=style/ImageCompress)

    再按一下 A，选择 Save Changes

    ![](https://img.mitsea.com/blog/posts/2023/04/FreeBSD%20%E6%9B%B4%E6%8D%A2%E4%B8%AD%E7%A7%91%E5%A4%A7%E8%BD%AF%E4%BB%B6%E5%8C%85%E6%BA%90/Untitled%201.png?x-oss-process=style/ImageCompress)

3. 更新索引

    ```bash
    pkg update -f
    ```
