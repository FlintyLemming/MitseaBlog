+++
author = "FlintyLemming"
title = "CDN 反代新浪图床"
slug = "d2b1364911ce4483b0089eb0a09c7309"
date = "2020-06-01"
description = ""
categories = ["Network", "MineService"]
tags = ["CDN", "Blog"]
image = "https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/06/CDN%20%E5%8F%8D%E4%BB%A3%E6%96%B0%E6%B5%AA%E5%9B%BE%E5%BA%8A/title.avif"
+++

新浪微博加防盗链有段时间了，但是允许空 Refer 访问，虽然在 Chrome 浏览器可以添加代码让图片正常加载，但是兼容性不好。于是就有了 CDN 反代新浪图床的方法。

## 配置过程
1. 阿里云 CDN 管理页面中添加一个 img.xxx.xxx 域名的 CDN，我的主域名是 flinty.moe. 所以这里就以 img.mitsea.com 为例

2. 由于新浪图床的地址本身就是一个 CDN 域名，所以“源站地址”里不能填新浪图床的域名，需要查询到新浪图床某个具体的 CDN IP 地址，端口选择 HTTPS

3. 添加好后，把 CNAME 配置到你的 NS 服务上

4. 在“回源设置”中打开回源 Host，域名填一个新浪的图床域名，随便哪个都行，比如我这里的 tva1.sinaimg.cn

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/06/CDN%20%E5%8F%8D%E4%BB%A3%E6%96%B0%E6%B5%AA%E5%9B%BE%E5%BA%8A/1.avif)

5. 回源 SNI 也设置一下，地址跟 HOST 地址一样

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/06/CDN%20%E5%8F%8D%E4%BB%A3%E6%96%B0%E6%B5%AA%E5%9B%BE%E5%BA%8A/2.avif)

6. 签一个这个图床 CDN 域名的证书，或者你域名的通配符证书，然后在“HTTPS设置”中把证书添加一下

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/06/CDN%20%E5%8F%8D%E4%BB%A3%E6%96%B0%E6%B5%AA%E5%9B%BE%E5%BA%8A/3.avif)

7. 打开 HTTP/2

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/06/CDN%20%E5%8F%8D%E4%BB%A3%E6%96%B0%E6%B5%AA%E5%9B%BE%E5%BA%8A/4.avif)

8. 打开 TLS 1.3，关闭 1.0 和 1.1

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/06/CDN%20%E5%8F%8D%E4%BB%A3%E6%96%B0%E6%B5%AA%E5%9B%BE%E5%BA%8A/5.avif)

9. 打开防盗链，注意把允许空 Refer 关闭，建议选择白名单，然后添加一个你需要放图片的地址，比如你的博客地址

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/06/CDN%20%E5%8F%8D%E4%BB%A3%E6%96%B0%E6%B5%AA%E5%9B%BE%E5%BA%8A/6.avif)

## 使用方式
1. 将图片上传至新浪图床，工具一大堆，你要用 Chrome 的话在商店随便搜就有，FireFox 我用的这个

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2020/06/CDN%20%E5%8F%8D%E4%BB%A3%E6%96%B0%E6%B5%AA%E5%9B%BE%E5%BA%8A/7.avif)

2. 不管你用的什么工具上传，请务必勾选 https

3. 上传时不需要管生成后的域名是什么，即使不是刚才回源配置里的 tva1.sinaimg.cn 也没有关系，你只需要把这一部分换成你配置的 CDN 域名就可以