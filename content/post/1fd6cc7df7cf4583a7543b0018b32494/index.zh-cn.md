+++
author = "FlintyLemming"
title = "【归档】iOS 原生 UIKit 开发处理键盘遮挡输入框问题"
slug = "1fd6cc7df7cf4583a7543b0018b32494"
date = "2020-01-16"
description = ""
categories = ["Apple", "Coding"]
tags = ["UIKit", "Swift"]
image = "https://image.mitsea.com/blog/posts/2020/01/iOS%20%E5%8E%9F%E7%94%9F%20UIKit%20%E5%BC%80%E5%8F%91%E5%A4%84%E7%90%86%E9%94%AE%E7%9B%98%E9%81%AE%E6%8C%A1%E8%BE%93%E5%85%A5%E6%A1%86%E9%97%AE%E9%A2%98/slava-auchynnikau-B5nCJC-KpvA-unsplash.avif"
+++

# 概要

目前项目中遇到小屏手机中弹出键盘时挡住输入框的问题，网上的方法我看都是要么修改当前 scrollview 的 offset，要么是修改 .view.frame 的y轴位置。但该方法在页面纵幅较短的情况下会出现问题，所以我这边使用修改约束的方法。

# 预想方法

通过修改最上方的元素到顶端的距离来实现需求

### Risks

动画做起来的话，没有移动 frame 来的简单，不能用 animated: true，得要自己写。当然我这边 base 里别人写好了。

### Open Questions

也许移动 frame 可行，但我这边就是会出现之前视频上出现的那个问题，没有找到解决方法。

不过修改约束也没有经过长时间的充分测试，还没发现什么隐性问题。

# 核心步骤

1. 在 xib 中找到约束，并拖入 view 文件中
    
    ![](https://image.mitsea.com/blog/posts/2020/01/iOS%20%E5%8E%9F%E7%94%9F%20UIKit%20%E5%BC%80%E5%8F%91%E5%A4%84%E7%90%86%E9%94%AE%E7%9B%98%E9%81%AE%E6%8C%A1%E8%BE%93%E5%85%A5%E6%A1%86%E9%97%AE%E9%A2%98/Untitled.avif)
    
2. 起个名字
    
    ```swift
    @IBOutlet weak var topConstraint: NSLayoutConstraint!
    ```
    
3. 在 ViewController 里计算应当修改的数值，并进行偏移
    
    ```swift
    @objc private func keyboardWillAppear(notification: NSNotification) {
        let keyboardRect = (notification.userInfo![UIResponder.keyboardFrameEndUserInfoKey] as! NSValue).cgRectValue
        let keyboardHeight = keyboardRect.size.height
        let screenHeight = self.view.frame.height
        let scrollViewOffset = cala001030View.scrollView.contentOffset.y
        let textFieldOffset = currentField.frame.origin.y - scrollViewOffset
        let visibleHeight = screenHeight - keyboardHeight - currentField.frame.size.height
           
        self.isNeedResetViewFrame = visibleHeight < textFieldOffset
        self.keyboardHeight = keyboardHeight
            
        let inputButtomToViewTop = currentField.frame.origin.y + currentField.frame.height + 30
        let keyboardTopToViewTop = screenHeight - keyboardHeight
        let shouldOffset = inputButtomToViewTop - keyboardTopToViewTop - scrollViewOffset
           
        if visibleHeight < textFieldOffset {
            self.cala001030View.topConstraint.constant = -shouldOffset
        }
    }
    ```
    
    关于偏移量是如何计算出来的，会单独开一个文章来说明
    
4. 键盘收回时，恢复原来的约束即可
    
    ```swift
    @objc private func keyboardWillHide(notification: NSNotification) {
        self.cala001030View.topConstraint.constant = 0
    }
    ```

> Photo by [Slava Auchynnikau](https://unsplash.com/@auchynnikau?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/the-sun-is-shining-through-the-clouds-over-the-mountains-B5nCJC-KpvA?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)
  