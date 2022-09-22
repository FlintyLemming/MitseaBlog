+++
author = "FlintyLemming"
title = "Windows Server 创建 Parity HDD 与 Mirror SSD 混合存储"
date = "2022-06-27"
description = ""
categories = ["Windows"]
tags = ["NAS"]
image = "https://img.mitsea.com/blog/posts/2022/06/Windows%20Server%20%E5%88%9B%E5%BB%BA%20Parity%20HDD%20%E4%B8%8E%20Mirror%20SSD%20%E6%B7%B7%E5%90%88%E5%AD%98%E5%82%A8/title.jpg?x-oss-process=style/ImageCompress"
+++

当尝试创建分层存储空间的时候，我发现如果勾选，则不能创建 Parity 分区

![](https://img.mitsea.com/blog/posts/2022/06/Windows%20Server%20%E5%88%9B%E5%BB%BA%20Parity%20HDD%20%E4%B8%8E%20Mirror%20SSD%20%E6%B7%B7%E5%90%88%E5%AD%98%E5%82%A8/1.png?x-oss-process=style/ImageCompress)

![](https://img.mitsea.com/blog/posts/2022/06/Windows%20Server%20%E5%88%9B%E5%BB%BA%20Parity%20HDD%20%E4%B8%8E%20Mirror%20SSD%20%E6%B7%B7%E5%90%88%E5%AD%98%E5%82%A8/2.png?x-oss-process=style/ImageCompress)

可能是因为我的 SSD 只有两块，所以只能通过 PowerShell 手动创建了

## 预览存储空间大小

因为后面有一个步骤需要输入 SSD 和 HDD 两个池分别设置多大，所以需要提前预览一下空间。比如可以先把所有的 HDD 创建一个存储池，然后新建虚拟磁盘，选择 Parity 后，看下这里的值是多少。

![](https://img.mitsea.com/blog/posts/2022/06/Windows%20Server%20%E5%88%9B%E5%BB%BA%20Parity%20HDD%20%E4%B8%8E%20Mirror%20SSD%20%E6%B7%B7%E5%90%88%E5%AD%98%E5%82%A8/3.png?x-oss-process=style/ImageCompress)

## 创建存储池

创建时，把 HDD 和 SSD 都勾选上

![](https://img.mitsea.com/blog/posts/2022/06/Windows%20Server%20%E5%88%9B%E5%BB%BA%20Parity%20HDD%20%E4%B8%8E%20Mirror%20SSD%20%E6%B7%B7%E5%90%88%E5%AD%98%E5%82%A8/4.png?x-oss-process=style/ImageCompress)

## 创建虚拟磁盘

1. 检查硬盘
    
    ```powershell
    Get-StoragePool <存储池名称> | Get-PhysicalDisk | Sort Size | FT -AutoSize
    ```
    
    ![](https://img.mitsea.com/blog/posts/2022/06/Windows%20Server%20%E5%88%9B%E5%BB%BA%20Parity%20HDD%20%E4%B8%8E%20Mirror%20SSD%20%E6%B7%B7%E5%90%88%E5%AD%98%E5%82%A8/5.png?x-oss-process=style/ImageCompress)
    
2. 创建高速存储
    
    ```powershell
    New-StorageTier -StoragePoolFriendlyName <存储池名称> -FriendlyName performance -MediaType SSD
    ```
    
    ![](https://img.mitsea.com/blog/posts/2022/06/Windows%20Server%20%E5%88%9B%E5%BB%BA%20Parity%20HDD%20%E4%B8%8E%20Mirror%20SSD%20%E6%B7%B7%E5%90%88%E5%AD%98%E5%82%A8/6.png?x-oss-process=style/ImageCompress)
    
3. 创建容量存储
    
    ```powershell
    New-StorageTier -StoragePoolFriendlyName <存储池名称> -ResiliencySettingName Parity -FriendlyName capacity -MediaType HDD
    ```
    
    ![](https://img.mitsea.com/blog/posts/2022/06/Windows%20Server%20%E5%88%9B%E5%BB%BA%20Parity%20HDD%20%E4%B8%8E%20Mirror%20SSD%20%E6%B7%B7%E5%90%88%E5%AD%98%E5%82%A8/7.png?x-oss-process=style/ImageCompress)
    
4. 创建混合卷
    
    注意这里容量的值可能要设置的比之前看的要小一些
    
    ```powershell
    New-Volume -FriendlyName <卷名称> -FileSystem ReFS -StoragePoolFriendlyName <存储池名称> -StorageTierFriendlyNames performance, capacity -StorageTierSizes <高速空间大小>, <容量空间大小>
    ```
    
    ![](https://img.mitsea.com/blog/posts/2022/06/Windows%20Server%20%E5%88%9B%E5%BB%BA%20Parity%20HDD%20%E4%B8%8E%20Mirror%20SSD%20%E6%B7%B7%E5%90%88%E5%AD%98%E5%82%A8/8.png?x-oss-process=style/ImageCompress)
    
5. 添加盘符
    
    ![](https://img.mitsea.com/blog/posts/2022/06/Windows%20Server%20%E5%88%9B%E5%BB%BA%20Parity%20HDD%20%E4%B8%8E%20Mirror%20SSD%20%E6%B7%B7%E5%90%88%E5%AD%98%E5%82%A8/9.png?x-oss-process=style/ImageCompress)
    
6. 然后在卷这边就能看到了，可以开启去重等功能
    
    ![](https://img.mitsea.com/blog/posts/2022/06/Windows%20Server%20%E5%88%9B%E5%BB%BA%20Parity%20HDD%20%E4%B8%8E%20Mirror%20SSD%20%E6%B7%B7%E5%90%88%E5%AD%98%E5%82%A8/10.png?x-oss-process=style/ImageCompress)