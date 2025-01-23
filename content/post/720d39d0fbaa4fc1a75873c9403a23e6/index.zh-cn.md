+++
author = "FlintyLemming"
title = "【归档】Android SMS 转发到 Telegram"
slug = "720d39d0fbaa4fc1a75873c9403a23e6"
date = "2020-02-08"
description = ""
categories = ["Android", "LifeTec"]
tags = ["Telegram"]
image = "https://image.mitsea.com/blog/posts/2020/02/Android%20SMS%20%E8%BD%AC%E5%8F%91%E5%88%B0%20Telegram/title.avif"
+++

项目地址：[https://github.com/qwe7002/telegram-sms](https://github.com/qwe7002/telegram-sms)

作者：qwe7002

最近购入了一部 Android 备用机，主要拿来开热点、收短信。但是一般都放在包里，如果需要查看两步验证短信就很麻烦。正好 qwe7002 也做了个工具，可以转发短信到 Telegram 上，这里简单介绍下配置步骤。

### 获取一只 Telegram Bot

1. 浏览器中打开这个网址：[https://telegram.me/botfather](https://telegram.me/botfather) ，会跳转到与 BotFather 的对话中。
2. 点击

        /start

    再点击

        /newbot

3. 接下来他会询问你要创建的机器人的名称(Alright, a new bot. How are we going to call it? Please choose a name for your bot.)，自己起一个名称
4. 然后会询问你机器人的ID，必须要以 "bot" 结尾(Good. Now let's choose a username for your bot. It must end in `bot`. Like this, for example: TetrisBot or tetris_bot.)，自己起一个，只能用英文
5. 然后他会给你一个 token，大概是这样，记下这一串字符，冒号左右一起的

        Use this token to access the HTTP API:
        xxxxxxxx:xxxxxxxxxxxxxx

### 配置App

1. 从文章开头的地址下载 Android 端 apk 安装包并安装，打开，如图

    ![](https://image.mitsea.com/blog/posts/2020/02/Android%20SMS%20%E8%BD%AC%E5%8F%91%E5%88%B0%20Telegram/1.avif)

2. 在“机器人令牌”处填写刚才获得的 token
3. 在 Telegram 上给机器人发一句话，内容任意（一句可能不行，多发几句x）
4. 回到 App ，点击 “获取最近的对话ID”，就会获得对话ID
5. “可信的电话号码”可以填写一个，用来使用短信操控 Bot
6. 下面可以选择附加功能，如“监控电池状态变动”，用来发送手机低电量和充电提醒
7. 最后点击“测试并保存”，Telegram Bot 上提示如下消息说明配置完毕

        [系统信息]
        您已成功连接到 Telegram bot 服务器。

### 其他

对机器人发送 /start 可以获取命令

    [系统信息]
    可用命令：
    /getinfo - 获取系统信息
    /sendsms - 发送短信
    /sendsms2 - 使用第二个卡槽发送短信

### 效果展示

![](https://image.mitsea.com/blog/posts/2020/02/Android%20SMS%20%E8%BD%AC%E5%8F%91%E5%88%B0%20Telegram/2.avif)
