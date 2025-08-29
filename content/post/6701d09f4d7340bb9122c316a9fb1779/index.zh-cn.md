+++
author = "FlintyLemming"
title = "SQL Server 数据库迁移"
slug = "6701d09f4d7340bb9122c316a9fb1779"
date = "2020-03-02"
description = ""
categories = ["Coding"]
tags = ["数据库"]
image = "https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/03/SQL%20Server%20%E6%95%B0%E6%8D%AE%E5%BA%93%E8%BF%81%E7%A7%BB/title.avif"
+++

## 备份原数据库

1. 首先备份原来的数据库，右键数据库 Tasks - Back Up...

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/03/SQL%20Server%20%E6%95%B0%E6%8D%AE%E5%BA%93%E8%BF%81%E7%A7%BB/1.avif)

2. Destination 里 Add... 选择一个本地路径

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/03/SQL%20Server%20%E6%95%B0%E6%8D%AE%E5%BA%93%E8%BF%81%E7%A7%BB/2.avif)

    **注意，这里的目录是你服务器所在机器的目录，不是你运行这个数据库 IDE 机器的的目录**

3. 备份完毕后，可以获得一个 .bak 的文件

## 还原数据库

1. 以默认 **Windows 身份验证** 的方式连接本地数据库

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/03/SQL%20Server%20%E6%95%B0%E6%8D%AE%E5%BA%93%E8%BF%81%E7%A7%BB/3.avif)

2. 在 **对象资源管理器** 中，右键 **数据库**，点击 **还原数据库**

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/03/SQL%20Server%20%E6%95%B0%E6%8D%AE%E5%BA%93%E8%BF%81%E7%A7%BB/4.avif)

3. 点击 **源** - **设备** 右边的选择按钮，打开 **选择备份设备** 窗口，点击 **添加** 选择之前备份的 .bak 文件，然后点击 **确定**

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/03/SQL%20Server%20%E6%95%B0%E6%8D%AE%E5%BA%93%E8%BF%81%E7%A7%BB/5.avif)

4. 选择后，在 **目标** 这里会自动显示备份文件所包含的数据库名称，最后点击 **确定**，进行还原

    ![](Shttps://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/03/SQL%20Server%20%E6%95%B0%E6%8D%AE%E5%BA%93%E8%BF%81%E7%A7%BB/6.avif)

## 设置账户

1. 右键 **安全性** - **登录名** 中的 **sa** 账户，打开 **属性**

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/03/SQL%20Server%20%E6%95%B0%E6%8D%AE%E5%BA%93%E8%BF%81%E7%A7%BB/7.avif)

2. 在这边设置密码

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/03/SQL%20Server%20%E6%95%B0%E6%8D%AE%E5%BA%93%E8%BF%81%E7%A7%BB/8.avif)

3. 在这边启用登录名，点击 **确定**

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/03/SQL%20Server%20%E6%95%B0%E6%8D%AE%E5%BA%93%E8%BF%81%E7%A7%BB/9.avif)

4. 这里右键，打开 **属性**

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/03/SQL%20Server%20%E6%95%B0%E6%8D%AE%E5%BA%93%E8%BF%81%E7%A7%BB/10.avif)

5. 这边选择 **SQL Server 和 Windows 身份验证模式**

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/03/SQL%20Server%20%E6%95%B0%E6%8D%AE%E5%BA%93%E8%BF%81%E7%A7%BB/11.avif)

6. 打开 **配置管理器**

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/03/SQL%20Server%20%E6%95%B0%E6%8D%AE%E5%BA%93%E8%BF%81%E7%A7%BB/12.avif)

7. 启用 Named Pipes 协议

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/03/SQL%20Server%20%E6%95%B0%E6%8D%AE%E5%BA%93%E8%BF%81%E7%A7%BB/13.avif)

8. 重启 SQL Server (MSSQLSERVER) 服务

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/03/SQL%20Server%20%E6%95%B0%E6%8D%AE%E5%BA%93%E8%BF%81%E7%A7%BB/14.avif)

## 开启远程访问（可选）

1. 同样是打开 **配置管理器**，然后找到 **MSSQLSERVER 的协议** 里 TCP/IP，把 **协议** 选项卡里的 **已启用** 改为 **是**

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/03/SQL%20Server%20%E6%95%B0%E6%8D%AE%E5%BA%93%E8%BF%81%E7%A7%BB/15.avif)

2. **IP 地址** 选项卡里，有好几个本地 IP 地址，看清楚需要的是那个，你要是开放到公网，就选跟路由器同一个网段的 IPv4 地址，这样后面设置 NAT 的时候不会出问题。

    找到正确的 IP 地址后，把下面的 **已启用** 改为 **是**。端口地址改不改无所谓，如果考虑安全性，后面路由器 NAT 设置的时候可以 NAT 到别的端口。

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/03/SQL%20Server%20%E6%95%B0%E6%8D%AE%E5%BA%93%E8%BF%81%E7%A7%BB/16.avif)

3. 设置好后，点击确定，会提示你重启服务，按要求重启下 SQLServer 的服务，跟上面是一样的。
4. 打开 **控制面板** 里的 **Windows Defender 防火墙**，左边打开 **高级设置**

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/03/SQL%20Server%20%E6%95%B0%E6%8D%AE%E5%BA%93%E8%BF%81%E7%A7%BB/17.avif)

5. 新建一个端口策略的入站规则

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/03/SQL%20Server%20%E6%95%B0%E6%8D%AE%E5%BA%93%E8%BF%81%E7%A7%BB/18.avif)

6. 填写 1433

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/03/SQL%20Server%20%E6%95%B0%E6%8D%AE%E5%BA%93%E8%BF%81%E7%A7%BB/19.avif)

7. 允许连接

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/03/SQL%20Server%20%E6%95%B0%E6%8D%AE%E5%BA%93%E8%BF%81%E7%A7%BB/20.avif)

8. 后面保持默认，完成规则添加。设置完毕。