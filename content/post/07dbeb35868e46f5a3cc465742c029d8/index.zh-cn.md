+++
author = "FlintyLemming"
title = "群晖 Docker 安装 qbittorrent 含 https 设置"
slug = "07dbeb35868e46f5a3cc465742c029d8"
date = "2020-02-13"
description = ""
categories = ["HomeLab"]
tags = ["Synology", "Docker"]
image = "https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/02/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qbittorrent%20%E5%90%AB%20https%20%E8%AE%BE%E7%BD%AE/title.avif"
+++

**教程已更新 DSM 7.2，请移步[这里](https://blog.mitsea.com/f34d4a3f981846469cac041c1df0881d/)**

## 速查

### 镜像名

linuxserver/qbittorrent

### 文件夹映射

    /path/to/appdata/config:/config
    /path/to/downloads:/downloads

### 端口映射

    -p 6881:6881 \
    -p 6881:6881/udp \
    -p 8080:8080 \

### 环境变量

    -e WEBUI_PORT=8080 \
    -e TempPath=/downloads \
    -e SavePath=/downloads \

## 具体步骤

### 安装 Docker

在 套件中心 找到并安装 Docker。注意，ARM 架构处理的群晖设备不支持 Docker

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/02/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qbittorrent%20%E5%90%AB%20https%20%E8%AE%BE%E7%BD%AE/Untitled.avif)

### 准备下载文件夹

实际上，需要准备一个下载文件夹和一个存储配置文件的文件夹。

1. 打开 File Station，找到一个合适的位置，创建一个名为 qbittorrent 的文件夹。然后在文件夹里面创建名为 config 和 downloads 文件夹。文件夹名字可以不跟我的一样，只要保证后面设置映射的时候能分清楚即可。

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/02/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qbittorrent%20%E5%90%AB%20https%20%E8%AE%BE%E7%BD%AE/Untitled%201.avif)

2. 右键 qbitorrent 这个文件夹，打开属性

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/02/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qbittorrent%20%E5%90%AB%20https%20%E8%AE%BE%E7%BD%AE/Untitled%202.avif)

3. 点击 权限 选项卡，然后点击 新增 按钮，打开 权限编辑器

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/02/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qbittorrent%20%E5%90%AB%20https%20%E8%AE%BE%E7%BD%AE/Untitled%203.avif)

4. 用户或组 选择 Everyone；下面的权限都勾上，然后点击确定

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/02/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qbittorrent%20%E5%90%AB%20https%20%E8%AE%BE%E7%BD%AE/Untitled%204.avif)

5. 然后勾选 应用到这个文件夹、子文件夹及文件，最后点击 确定 即可。

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/02/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qbittorrent%20%E5%90%AB%20https%20%E8%AE%BE%E7%BD%AE/Untitled%205.avif)

### 下载并配置镜像

1. 打开 Docker，左边打开 注册表，搜索 qbittorrent，找到 linuxserver/qbittorrent 这个镜像

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/02/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qbittorrent%20%E5%90%AB%20https%20%E8%AE%BE%E7%BD%AE/Untitled%206.avif)

2. 双击，稍等后会让你选择版本，保持默认的 latest，最新的版本

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/02/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qbittorrent%20%E5%90%AB%20https%20%E8%AE%BE%E7%BD%AE/Untitled%207.avif)

3. 在左侧 映像 选项卡里可以看到正在下载镜像，等图标不再闪烁后，或 启动 按钮可点击时，则表示下载好。由于服务器不在中国大陆，所以可能需要等较长一段时间，完整大小大约 310M。

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/02/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qbittorrent%20%E5%90%AB%20https%20%E8%AE%BE%E7%BD%AE/Untitled%208.avif)

4. 下载完毕后，双击，或者点击 启动 按钮。进入启动前的配置页面

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/02/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qbittorrent%20%E5%90%AB%20https%20%E8%AE%BE%E7%BD%AE/Untitled%209.avif)

    容器名称 可以自定义一个好认的名字。然后点击 高级设置。（不要点下一步）

5. 打开 卷 选项卡，这里要设置目录的映射。即运行时，容器内部的文件夹映射到外面，即实际文件目录的位置。

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/02/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qbittorrent%20%E5%90%AB%20https%20%E8%AE%BE%E7%BD%AE/Untitled%2010.avif)

6. 要创建两个文件夹映射，分别对应之前创建的两个文件夹。先创建 config 文件夹的映射。点击 添加文件夹 按钮，在弹出的窗口选择刚才创建的 config 文件夹

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/02/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qbittorrent%20%E5%90%AB%20https%20%E8%AE%BE%E7%BD%AE/Untitled%2011.avif)

    选中后，点击 选择 按钮。然后在 装载路径 里填写

        /config

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/02/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qbittorrent%20%E5%90%AB%20https%20%E8%AE%BE%E7%BD%AE/Untitled%2012.avif)

    - 至于为什么填写 /config，可以在这个镜像的[说明页面](https://registry.hub.docker.com/r/linuxserver/qbittorrent)里查到

        其中写到

            volumes:
                  - /path/to/appdata/config:/config
                  - /path/to/downloads:/downloads

        docker 配置中，冒号左边的是外面的目录，冒号右边的是容器里面的目录，所以我们这里要填写 /config

    同理，按照相同的方式，建立 downloads 文件的映射，最后就是这样子

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/02/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qbittorrent%20%E5%90%AB%20https%20%E8%AE%BE%E7%BD%AE/Untitled%2013.avif)

7. 点击 端口设置 选项卡，修改端口设置

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/02/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qbittorrent%20%E5%90%AB%20https%20%E8%AE%BE%E7%BD%AE/Untitled%2014.avif)

    这里可以看到，容器里默认配置了 6881 和 8080，这分别是 bt 端口和 WebUI 端口。由于 6881 这个端口作为默认端口可能被封，所以应该换成别的端口。之前看教程别人用的 52000，我用着没问题，所以就改成 52000。8080 的 WebUI 端口也可以换一个自己需要的，我这里设置为 8848。

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/02/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qbittorrent%20%E5%90%AB%20https%20%E8%AE%BE%E7%BD%AE/Untitled%2015.avif)

    bt 端口改的话，最好内外都改，不然客户端汇报给 tracker 的端口和实际的不一致，可能会出问题。WebUI 端口可以只设置个映射出来的本地端口，但这样的话，你后面配置环境变量的时候，WEBUI_PORT 参数就还保持原来的 8080。不过我这边保持统一就都改了。

8. 点击 环境 选项卡，设置环境变量。这里要添加三个环境变量，内容如下。

        可变          值
        TempPath     /downloads
        SavePath     /downloads
        WEBUI_PORT   8848

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/02/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qbittorrent%20%E5%90%AB%20https%20%E8%AE%BE%E7%BD%AE/Untitled%2016.avif)

    最后点击应用

9. 点击 下一步，简单确认下设置项，没问题就可以点 应用，启动容器了。

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/02/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qbittorrent%20%E5%90%AB%20https%20%E8%AE%BE%E7%BD%AE/Untitled%2017.avif)

### 使用 qbittorrent

1. 启动后，就可以在 容器 选项卡看到正在运行的容器了

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/02/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qbittorrent%20%E5%90%AB%20https%20%E8%AE%BE%E7%BD%AE/Untitled%2018.avif)

2. 点击它，然后点击 详情，点开 日志 选项卡，查看日志

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/02/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qbittorrent%20%E5%90%AB%20https%20%E8%AE%BE%E7%BD%AE/Untitled%2019.avif)

    如果你配置时出现问题导致 qbittorrent 无法启动，会在这里显示错误信息。如果一切正常，则会看到进入 qbittorrent 的 WebUI 的地址、用户名和密码。

3. 打开 <NAS的本地IP>:8848，即可打开 qbittorrent 的 WebUI 后台登录界面

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/02/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qbittorrent%20%E5%90%AB%20https%20%E8%AE%BE%E7%BD%AE/Untitled%2020.avif)

4. 输入在 log 里看到的默认账号 admin，密码 adminadmin，即可进入后台。第一件事就是改密码。打开设置 - Web UI，在 Authentication 里，Password 填写新密码即可

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/02/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qbittorrent%20%E5%90%AB%20https%20%E8%AE%BE%E7%BD%AE/Untitled%2021.avif)

5. 同时，还可以在 Language 里修改显示语言

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/02/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qbittorrent%20%E5%90%AB%20https%20%E8%AE%BE%E7%BD%AE/Untitled%2022.avif)

    之后就可以正常使用了

## 其他

### 设置 https

#### 内置 https 设置

1. 设置 - WebUI - Web 用户界面（远程控制），勾选 使用 HTTPS 而不是 HTTP
2. 打开下面的 证书信息 链接，可以看到跳到了 Apache 的说明页面。通过查看说明

        SSLCertificateFile    "/path/to/this/server.crt"
        SSLCertificateKeyFile "/path/to/this/server.key"

    可以看到需要 .crt 和 .key 两个格式的证书。这就看你证书颁发商给你什么格式的了，如果给的是 Nginx 用的那种，crt 文件内容是多的，需要自己处理下，这里就不详细讲了。我是腾讯云弄的证书，直接给了 Apache 使用的证书。包含如下三个文件，这里只需要后两个。

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/02/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qbittorrent%20%E5%90%AB%20https%20%E8%AE%BE%E7%BD%AE/Untitled%2023.avif)

3. 将这两个文件放到 config 文件夹里

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/02/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qbittorrent%20%E5%90%AB%20https%20%E8%AE%BE%E7%BD%AE/Untitled%2024.avif)

4. qbittorrent 设置里，对应填写如下内容

        证书  /config/2_domain.crt
        秘钥  /config/3_domain.key

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/02/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qbittorrent%20%E5%90%AB%20https%20%E8%AE%BE%E7%BD%AE/Untitled%2025.avif)

5. 保存后，试一下通过 https + 域名 + 端口 的方式访问下，没问题

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/02/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qbittorrent%20%E5%90%AB%20https%20%E8%AE%BE%E7%BD%AE/Untitled%2026.avif)

#### 群晖内置的反代工具设置

其实我更推荐这种方式，因为这样方便管理。不过你通过 https 访问的端口就必须和 WebUI 的端口不一样了，不然会冲突。具体设置方法，可以参考我[另一篇教程](https://blog.mitsea.com/4a08e5064d834921b206bf128e463d1a/)。

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/02/%E7%BE%A4%E6%99%96%20Docker%20%E5%AE%89%E8%A3%85%20qbittorrent%20%E5%90%AB%20https%20%E8%AE%BE%E7%BD%AE/Untitled%2027.avif)