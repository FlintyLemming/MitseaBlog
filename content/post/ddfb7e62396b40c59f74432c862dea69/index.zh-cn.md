+++
author = "FlintyLemming"
title = "使用 cli 登陆公司上网验证系统"
slug = "ddfb7e62396b40c59f74432c862dea69"
date = "2020-11-09"
categories = ["Linux", "Network"]
tags = ["Ubuntu"]
image = "https://img.mitsea.com/blog/posts/2020/11/%E4%BD%BF%E7%94%A8%20cli%20%E7%99%BB%E9%99%86%E5%85%AC%E5%8F%B8%E4%B8%8A%E7%BD%91%E9%AA%8C%E8%AF%81%E7%B3%BB%E7%BB%9F/pawel-wieladek-QpWb8mB1tVU-unsplash.jpg?x-oss-process=style/ImageCompress"
+++

最近需要尝试在 Ubuntu 上部署一个项目，装好 Ubuntu Server 就傻眼了，公司有个上网登录窗，我不知道怎么登陆。于是想到了三个方法：

第一，安装桌面环境，这选择很多啊，要么就是直接把 gnome 给装回来；要么就在 Docker 里跑一个 LXQt，那里面有个浏览器

第二，我记得之前又看到过很扭曲的东西，就是通过 cli 使用浏览器，是那种模拟图形界面的，但我感觉过于扭曲…或者经过群友介绍，知道有 headless Chrome 这么个东西，关联的东西有 puppeteer

第三，网页上点击登陆无非就是发个简单的请求，curl 命令也许就能搞定

于是我决定先试试 curl。

首先看下公司的网络登录窗，貌似就是深信服的标准登陆页面

![](https://img.mitsea.com/blog/posts/2020/11/%E4%BD%BF%E7%94%A8%20cli%20%E7%99%BB%E9%99%86%E5%85%AC%E5%8F%B8%E4%B8%8A%E7%BD%91%E9%AA%8C%E8%AF%81%E7%B3%BB%E7%BB%9F/Untitled.png?x-oss-process=style/ImageCompress)

用开发者选项查看源代码，可以在 /ac_portal/share/res/js/logic_new.js 里看到登陆的 js 方法

```jsx
function onPwdLogin(){
	if(!pwdValidtor())return;
	var params = {
		opr:'pwdLogin',//smsLogin表示短信认证登录,pwdLogin表示密码认证登录
		userName : $id("password_name").value,
		pwd : $id("password_pwd").value,
		rememberPwd : $id("rememberPwd").checked ? '1':'0'
	};
	loginRequest(params,"mode_password",$id("password_submitBtn"));
}
```

这样我们就得到了登陆请求的结构了，只需要构造这样一条命令即可

```shell
curl -d "opr=pwdLogin&userName=<用户名，要带引号>&pwd=<密码，要带引号>&rememberPwd=1" http://<公司上网登录窗的地址>/ac_portal/login.php
```

提示 logon success，成功

![](https://img.mitsea.com/blog/posts/2020/11/%E4%BD%BF%E7%94%A8%20cli%20%E7%99%BB%E9%99%86%E5%85%AC%E5%8F%B8%E4%B8%8A%E7%BD%91%E9%AA%8C%E8%AF%81%E7%B3%BB%E7%BB%9F/Untitled%201.png?x-oss-process=style/ImageCompress)

由于 curl 命令在几乎所有平台都可以用，所以可以自己设定一个开机自启的任务自动执行这么个命令，就不用每次上班打开电脑还要手动登陆了。

> Photo by [Paweł Wielądek](https://unsplash.com/@pawelwieladek?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
  