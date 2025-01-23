+++
author = "FlintyLemming"
title = "Xcode 里 App 的多环境管理"
slug = "3f3589d42a244fad8bb293185db8514f"
date = "2022-10-12"
description = ""
categories = ["Coding", "Apple"]
tags = ["Xcode"]
image = "https://image.mitsea.com/blog/posts/2022/10/Xcode%20%E9%87%8C%20App%20%E7%9A%84%E5%A4%9A%E7%8E%AF%E5%A2%83%E7%AE%A1%E7%90%86/title.avif"
+++

软件开发中一般需要对应不同的环境发布不同的成品，比如 Dev、Prod 等。通过使用 Xcode 的 scheme 可以管理多个环境，并在不同的环境下执行不同的行为。

首先需要明白两个概念，Scheme 和 Configuration。上面这个叫 Schema，下面的是 Configuration。

![](https://image.mitsea.com/blog/posts/2022/10/Xcode%20%E9%87%8C%20App%20%E7%9A%84%E5%A4%9A%E7%8E%AF%E5%A2%83%E7%AE%A1%E7%90%86/Untitled.avif)

你可以简单理解为包含关系，每个 Schema 里的 Debug、Build 等行为配置是参考 Configuration 里的项目。

## 创建 Configurations

1. 点击项目 Info 里 Configurations 下面的加号，可以以默认的 Debug 和 Release 为模板，创建多个 Configurations

    ![](https://image.mitsea.com/blog/posts/2022/10/Xcode%20%E9%87%8C%20App%20%E7%9A%84%E5%A4%9A%E7%8E%AF%E5%A2%83%E7%AE%A1%E7%90%86/Untitled%201.avif)

2. 每个环境里的 Debug 和 Release 还是要区分开的，所以这边创建出了如下的 Configurations。默认的那一组 Debug 和 Release 就当内部本地用，但是考虑到 Xcode 的草台性，不建议改它名字。

    ![](https://image.mitsea.com/blog/posts/2022/10/Xcode%20%E9%87%8C%20App%20%E7%9A%84%E5%A4%9A%E7%8E%AF%E5%A2%83%E7%AE%A1%E7%90%86/Untitled%202.avif)

3. 为了在代码里能够让代码根据不同的环境执行不同的行为，我们还需要给刚才创建的 Configurations 设置 Flag。在 Build Settings 里找到 Swift Compiler - Custom Flags 里的 Active Compilation Conditions。

    ![](https://image.mitsea.com/blog/posts/2022/10/Xcode%20%E9%87%8C%20App%20%E7%9A%84%E5%A4%9A%E7%8E%AF%E5%A2%83%E7%AE%A1%E7%90%86/Untitled%203.avif)

4. 最后就是这样

    ![](https://image.mitsea.com/blog/posts/2022/10/Xcode%20%E9%87%8C%20App%20%E7%9A%84%E5%A4%9A%E7%8E%AF%E5%A2%83%E7%AE%A1%E7%90%86/Untitled%204.avif)

## 创建 Scheme

创建完 Configurations 还不够，因为我们还不能很方便的去控制当前我们编译 App 的时候到底用哪套 Configuration，这个时候就需要创建多个 Scheme

1. 点击这里的 Manage Schemes

    ![](https://image.mitsea.com/blog/posts/2022/10/Xcode%20%E9%87%8C%20App%20%E7%9A%84%E5%A4%9A%E7%8E%AF%E5%A2%83%E7%AE%A1%E7%90%86/Untitled%205.avif)

2. 创建多个 Scheme，可以把默认的改成 Local

    ![](https://image.mitsea.com/blog/posts/2022/10/Xcode%20%E9%87%8C%20App%20%E7%9A%84%E5%A4%9A%E7%8E%AF%E5%A2%83%E7%AE%A1%E7%90%86/Untitled%206.avif)

3. 双击编辑，比如双击这里的 MyApp-Dev。Run 里的这个 Build Configuration 就要改成刚才创建的 Debug Dev

    ![](https://image.mitsea.com/blog/posts/2022/10/Xcode%20%E9%87%8C%20App%20%E7%9A%84%E5%A4%9A%E7%8E%AF%E5%A2%83%E7%AE%A1%E7%90%86/Untitled%207.avif)

4. 这五个行为里面的都要改，比如 Release 要改成刚才创建的 Release Dev

    ![](https://image.mitsea.com/blog/posts/2022/10/Xcode%20%E9%87%8C%20App%20%E7%9A%84%E5%A4%9A%E7%8E%AF%E5%A2%83%E7%AE%A1%E7%90%86/Untitled%208.avif)

5. 同理，另一个 MyApp-Prod 的 scheme 里也要都改掉，这样当我们点击这里切换的时候，就会使用不同的 Configuration

    ![](https://image.mitsea.com/blog/posts/2022/10/Xcode%20%E9%87%8C%20App%20%E7%9A%84%E5%A4%9A%E7%8E%AF%E5%A2%83%E7%AE%A1%E7%90%86/Untitled%209.avif)

## 编辑不同环境下的行为

### 代码逻辑

可以使用下面的这种语法为不同环境编写不同的逻辑代码

```swift
#if LOCAL
demoText.text = "Now is Local Environment"
#elseif DEV
demoText.text = "Now is Dev Environment"
#elseif PROD
demoText.text = "Now is Prod Environment"
#endif
```

看下效果

![](https://image.mitsea.com/blog/posts/2022/10/Xcode%20%E9%87%8C%20App%20%E7%9A%84%E5%A4%9A%E7%8E%AF%E5%A2%83%E7%AE%A1%E7%90%86/Untitled%2010.avif)

![](https://image.mitsea.com/blog/posts/2022/10/Xcode%20%E9%87%8C%20App%20%E7%9A%84%E5%A4%9A%E7%8E%AF%E5%A2%83%E7%AE%A1%E7%90%86/Untitled%2011.avif)

### 包名

不同环境包名可能都是不一样的。在 Siging & Capabilities 里，可以看到这里已经有刚才创建的选项了，只需要点击对应的选项，修改里面的 Bundle Identifier 即可。

![](https://image.mitsea.com/blog/posts/2022/10/Xcode%20%E9%87%8C%20App%20%E7%9A%84%E5%A4%9A%E7%8E%AF%E5%A2%83%E7%AE%A1%E7%90%86/Untitled%2012.avif)

### 显示名称

不同环境的显示名称也可能不一样，在 General 里修改

![](https://image.mitsea.com/blog/posts/2022/10/Xcode%20%E9%87%8C%20App%20%E7%9A%84%E5%A4%9A%E7%8E%AF%E5%A2%83%E7%AE%A1%E7%90%86/Untitled%2013.avif)

此外还有很多可以区分环境设置，包括代码中引用的变量、App Icon 等等……

> Photo by [Skylar McKissack](https://unsplash.com/@skymckissack?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
  