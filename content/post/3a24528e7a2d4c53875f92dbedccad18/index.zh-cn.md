+++
author = "FlintyLemming"
title = "SourceTree 跳过登录以及创建快捷方式"
slug = "3a24528e7a2d4c53875f92dbedccad18"
date = "2020-05-31"
description = ""
categories = ["Coding"]
tags = ["Git", "SourceTree"]
image = "https://img.mitsea.com/blog/posts/2020/05/SourceTree%20%E8%B7%B3%E8%BF%87%E7%99%BB%E5%BD%95%E4%BB%A5%E5%8F%8A%E5%88%9B%E5%BB%BA%E5%BF%AB%E6%8D%B7%E6%96%B9%E5%BC%8F/title.avif"
+++

## 跳过登录

**实测 3.3.8 有效**

1. 打开如下文件夹

        C:\Users\<你的用户名>\AppData\Local\Atlassian\SourceTree

2. 创建一个名为 accounts.json 文件，内容如下

        [
          {
            "$id": "1",
            "$type": "SourceTree.Api.Host.Identity.Model.IdentityAccount, SourceTree.Api.Host.Identity",
            "Authenticate": true,
            "HostInstance": {
              "$id": "2",
              "$type": "SourceTree.Host.Atlassianaccount.AtlassianAccountInstance, SourceTree.Host.AtlassianAccount",
              "Host": {
                "$id": "3",
                "$type": "SourceTree.Host.Atlassianaccount.AtlassianAccountHost, SourceTree.Host.AtlassianAccount",
                "Id": "atlassian account"
              },
              "BaseUrl": "https://id.atlassian.com/"
            },
            "Credentials": {
              "$id": "4",
              "$type": "SourceTree.Model.BasicAuthCredentials, SourceTree.Api.Account",
              "Username": "",
              "Email": null
            },
            "IsDefault": false
          }
        ]

    ![](https://img.mitsea.com/blog/posts/2020/05/SourceTree%20%E8%B7%B3%E8%BF%87%E7%99%BB%E5%BD%95%E4%BB%A5%E5%8F%8A%E5%88%9B%E5%BB%BA%E5%BF%AB%E6%8D%B7%E6%96%B9%E5%BC%8F/1.avif)

3. 然后进入下面这个文件夹

        C:\Users\<你的用户名>\AppData\Local\Atlassian\SourceTree.exe_Url_xxxxxxxx\<版本号>

4. 打开 user.config，添加如下一段配置

        <setting name="AgreedToEULA" serializeAs="String">
            <value>True</value>
        </setting>
        <setting name="AgreedToEULAVersion" serializeAs="String">
            <value>20160201</value>
        </setting>

    ![](https://img.mitsea.com/blog/posts/2020/05/SourceTree%20%E8%B7%B3%E8%BF%87%E7%99%BB%E5%BD%95%E4%BB%A5%E5%8F%8A%E5%88%9B%E5%BB%BA%E5%BF%AB%E6%8D%B7%E6%96%B9%E5%BC%8F/2.avif)

5. 修改完后重新运行安装程序即可

## 创建快捷方式

SourceTree 在安装完毕后往往不会生成桌面快捷方式和文件夹，需要自己手动找到文件目录，然后创建快捷方式。

文件目录一般是在下面这个文件夹里

    C:\Users\<你的用户名>\AppData\Local\SourceTree\app-<版本号>

或者你可以按照如下方式自己找

1. 运行 SourceTree 时，打开文件管理器。在 详细信息（Windows 7 里叫 进程）里找到 SourceTree

    ![](https://img.mitsea.com/blog/posts/2020/05/SourceTree%20%E8%B7%B3%E8%BF%87%E7%99%BB%E5%BD%95%E4%BB%A5%E5%8F%8A%E5%88%9B%E5%BB%BA%E5%BF%AB%E6%8D%B7%E6%96%B9%E5%BC%8F/3.avif)

2. 右键，点击 打开文件所在位置

    ![](https://img.mitsea.com/blog/posts/2020/05/SourceTree%20%E8%B7%B3%E8%BF%87%E7%99%BB%E5%BD%95%E4%BB%A5%E5%8F%8A%E5%88%9B%E5%BB%BA%E5%BF%AB%E6%8D%B7%E6%96%B9%E5%BC%8F/4.avif)

3. 这样也是能找到 SourceTree 的安装目录

    ![](https://img.mitsea.com/blog/posts/2020/05/SourceTree%20%E8%B7%B3%E8%BF%87%E7%99%BB%E5%BD%95%E4%BB%A5%E5%8F%8A%E5%88%9B%E5%BB%BA%E5%BF%AB%E6%8D%B7%E6%96%B9%E5%BC%8F/5.avif)

4. 右键 - 发送到 - 桌面快捷方式

    ![](https://img.mitsea.com/blog/posts/2020/05/SourceTree%20%E8%B7%B3%E8%BF%87%E7%99%BB%E5%BD%95%E4%BB%A5%E5%8F%8A%E5%88%9B%E5%BB%BA%E5%BF%AB%E6%8D%B7%E6%96%B9%E5%BC%8F/6.avif)
