+++
author = "FlintyLemming"
title = "【已归档】UIKit 正确添加 Scrollview"
slug = "0353b001d775429ea4ab23c511db6382"
date = "2019-12-04"
description = ""
categories = ["Apple", "Coding"]
tags = ["Word"]
image = "https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2019/12/UIKit%20%E6%AD%A3%E7%A1%AE%E6%B7%BB%E5%8A%A0%20Scrollview/sichen-xiang-0prBYAk2emU-unsplash.avif"
+++

1. 拖一个 Scrollview 到默认的 view 上，拖过来之后可以拉大一点，反正现在这个 Scrollview 的宽高是不确定的
    
    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2019/12/UIKit%20%E6%AD%A3%E7%A1%AE%E6%B7%BB%E5%8A%A0%20Scrollview/Untitled.avif)
    
2. 设置上下左右约束为 0
    
    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2019/12/UIKit%20%E6%AD%A3%E7%A1%AE%E6%B7%BB%E5%8A%A0%20Scrollview/Untitled%201.avif)
    
    💡 这里的 Constrain to margins 不能勾选
    
3. 拉一个 uiview 到 Scrollview 里
    
    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2019/12/UIKit%20%E6%AD%A3%E7%A1%AE%E6%B7%BB%E5%8A%A0%20Scrollview/Untitled%202.avif)
    
4. 从 View 拉到 Scrollview 的 Content Layout Guide
    
    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2019/12/UIKit%20%E6%AD%A3%E7%A1%AE%E6%B7%BB%E5%8A%A0%20Scrollview/Untitled%203.avif)
    
5. 把这四个全部点上 
    
    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2019/12/UIKit%20%E6%AD%A3%E7%A1%AE%E6%B7%BB%E5%8A%A0%20Scrollview/Untitled%204.avif)
    
6. 从 View 拖到 Frame Layout Guide
    
    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2019/12/UIKit%20%E6%AD%A3%E7%A1%AE%E6%B7%BB%E5%8A%A0%20Scrollview/Untitled%205.avif)
    
7. 设置等宽
    
    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2019/12/UIKit%20%E6%AD%A3%E7%A1%AE%E6%B7%BB%E5%8A%A0%20Scrollview/Untitled%206.avif)
    
8. 看下刚刚创建的五个约束，有些固定的值
    
    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2019/12/UIKit%20%E6%AD%A3%E7%A1%AE%E6%B7%BB%E5%8A%A0%20Scrollview/Untitled%207.avif)
    
9. 依次点击，然后在右边把值都改成0。比如这里 159 要改成 0
    
    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2019/12/UIKit%20%E6%AD%A3%E7%A1%AE%E6%B7%BB%E5%8A%A0%20Scrollview/Untitled%208.avif)
    
10. 对于有比例的这一项，改成1。比如这里的 0.57971 改成 1
    
    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2019/12/UIKit%20%E6%AD%A3%E7%A1%AE%E6%B7%BB%E5%8A%A0%20Scrollview/Untitled%209.avif)
    
    到此为止，左右都设定好了，还有上下的高度没有设定。上下的高度需要靠 view 里具体的元素到 view 的上下高度决定，然后进一步决定整个 Scrollview 的高度
    
11. 随便拖一个元素到 view 里，比如一个 label
    
    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2019/12/UIKit%20%E6%AD%A3%E7%A1%AE%E6%B7%BB%E5%8A%A0%20Scrollview/Untitled%2010.avif)
    
12. 设置下到上下高度的约束，尤其是到下的，可以设置大一点
    
    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2019/12/UIKit%20%E6%AD%A3%E7%A1%AE%E6%B7%BB%E5%8A%A0%20Scrollview/Untitled%2011.avif)
    
13. 设置完后可以看到两条蓝色的 view 边线延伸下来，说明设置成功，此时页面也可以滚动
    
    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2019/12/UIKit%20%E6%AD%A3%E7%A1%AE%E6%B7%BB%E5%8A%A0%20Scrollview/Untitled%2012.avif)

> Photo by [Sichen Xiang](https://unsplash.com/@imseason?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/a-large-room-with-lots-of-windows-in-it-0prBYAk2emU?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)
  