+++
author = "FlintyLemming"
title = "使用 Bitwarden 管理 SSH 秘钥和连接"
slug = "cS6AsVzKn51Ao3ntJEN6q7"
date = "2025-12-14"
description = "感觉非常的先进.jpg"
categories = ["Linux"]
tags = ["SSH", "Bitwarden"]
image = "https://assets.mitsea.cn/blog/posts/2025/12/%E4%BD%BF%E7%94%A8%20Bitwarden%20%E7%AE%A1%E7%90%86%20SSH%20%E7%A7%98%E9%92%A5%E5%92%8C%E8%BF%9E%E6%8E%A5/brooke-balentine-sv55rtLoDVY-unsplash.avif"
+++

## 背景

最近各种 GUI 的 SSH 客户端用腻了，先前一直用的 xterminal，功能上倒是非常好用，但是它那个 Electron 性能优化的太差了，AMD 核显渲染也有很多问题，于是就想干脆直接用 ssh config 来管理 SSH 连接了。不过 ssh config 要配置免密登录的话，ssh 秘钥就得放在 \~/.ssh 下，感觉特别不安全，特别是 macOS 和 Windows，别的程序都有权限可以查看。

查了下现在主流的密码管理工具都有 SSH Agent 的功能了，可以存储 SSH 密钥对，并且在登录的时候可以调用。这样就不需要把秘钥文件放在文件系统里了，听起来是非常的安全.jpg

1Password 什么的应该也类似，可以参考

## 配置方式

### Bitwarden

设置里开启 SSH Agent

![](https://assets.mitsea.cn/blog/posts/2025/12/%E4%BD%BF%E7%94%A8%20Bitwarden%20%E7%AE%A1%E7%90%86%20SSH%20%E7%A7%98%E9%92%A5%E5%92%8C%E8%BF%9E%E6%8E%A5/image_MEV5OkpyN4.avif)

然后创建密钥对

![](https://assets.mitsea.cn/blog/posts/2025/12/%E4%BD%BF%E7%94%A8%20Bitwarden%20%E7%AE%A1%E7%90%86%20SSH%20%E7%A7%98%E9%92%A5%E5%92%8C%E8%BF%9E%E6%8E%A5/image_b4Oq3lwH7_.avif)

按照图中步骤就可以，他会帮你生成密钥对并且无法修改

### 服务器

1. 添加公钥，复制 Bitwarden 里生成的公钥，然后编辑文件添加
   ```bash 
   # 把剪贴板内容粘到文件尾部新起一行
   nano ~/.ssh/authorized_keys
   chmod 600 ~/.ssh/authorized_keys
   ```

2. 服务器端 SSH 守护进程检查（只做一次，以后不用再动） &#x20;

   编辑 `/etc/ssh/sshd_config`，确保下面三行未被注释且值正确：
   ```bash 
   PubkeyAuthentication yes
   AuthorizedKeysFile .ssh/authorized_keys
   PasswordAuthentication no   # 为了强制只用密钥，可选
   ```

3. 重启服务
   ```bash 
   sudo systemctl restart sshd  # CentOS/Rocky/Alma
   sudo systemctl restart ssh  # Ubuntu/Debian
   ```


### 客户端

在本地用户目录下的 .ssh 文件夹里创建 config 文件（不要有后缀）内容如下

```markdown 
Host <连接名称，随便写>
    HostName <服务器 IP>
    User <登录用户>
    Port <连接端口>
    IdentityFile <公钥文件配置>
    IdentitiesOnly yes
```


最后看起来像这样，IdentityFile 那个文件里就写你这个机器的公钥，它充当“指路牌”的功能，这样 SSH 客户端就能与服务器的公钥进行匹配，直接在 SSH Agent 里拿对应的私钥登录。如果不设置，由于 SSH 客户端不知道该用哪个秘钥登录，他就会挨个都尝试一遍，当你 Bitwarden 里存了超过 6 个秘钥时，就有可能超过服务器设置的尝试次数。

![image](https://assets.mitsea.cn/blog/posts/2025/12/%E4%BD%BF%E7%94%A8%20Bitwarden%20%E7%AE%A1%E7%90%86%20SSH%20%E7%A7%98%E9%92%A5%E5%92%8C%E8%BF%9E%E6%8E%A5/image_54BiABuGx1.avif)

### macOS 特别注意

macOS 有点特殊的是它系统默认会起一个 SSH Agent，可以通过 `echo $SSH_AUTH_SOCK` 查看

![](https://assets.mitsea.cn/blog/posts/2025/12/%E4%BD%BF%E7%94%A8%20Bitwarden%20%E7%AE%A1%E7%90%86%20SSH%20%E7%A7%98%E9%92%A5%E5%92%8C%E8%BF%9E%E6%8E%A5/image_b4ArDsBmSg.avif)

所以需要修改默认的 SSH Agent 为 Bitwarden 的，执行下面的命令即可

```bash 
export SSH_AUTH_SOCK="$HOME/Library/Containers/com.bitwarden.desktop/Data/.bitwarden-ssh-agent.sock"
```


这样就可以看到 Bitwarden 里面的秘钥了，确认没问题的话，把这个环境变量加到 shell 的配置文件里即可

![](https://assets.mitsea.cn/blog/posts/2025/12/%E4%BD%BF%E7%94%A8%20Bitwarden%20%E7%AE%A1%E7%90%86%20SSH%20%E7%A7%98%E9%92%A5%E5%92%8C%E8%BF%9E%E6%8E%A5/image_AKfAW6o-1I.avif)

## 使用 SSH Agent 登录

通过 `ssh <刚才写的 Host 名>` 连接服务器后，Bitwarden 就会弹出窗口，请求批准登录

![](https://assets.mitsea.cn/blog/posts/2025/12/%E4%BD%BF%E7%94%A8%20Bitwarden%20%E7%AE%A1%E7%90%86%20SSH%20%E7%A7%98%E9%92%A5%E5%92%8C%E8%BF%9E%E6%8E%A5/image_obMgH5KtcV.avif)

点击批准就可以登录了，整体上感觉还是比较简单的

## 后续优化

将“使用 SSH 代理时提示授权”改成从不，这样每次 SSH 登录的时候就不需要点批准了

![](https://assets.mitsea.cn/blog/posts/2025/12/%E4%BD%BF%E7%94%A8%20Bitwarden%20%E7%AE%A1%E7%90%86%20SSH%20%E7%A7%98%E9%92%A5%E5%92%8C%E8%BF%9E%E6%8E%A5/iShot_2025-12-19_21.18.07.avif)

配合 fish 的预测，丝滑登录

![](<https://assets.mitsea.cn/blog/posts/2025/12/%E4%BD%BF%E7%94%A8%20Bitwarden%20%E7%AE%A1%E7%90%86%20SSH%20%E7%A7%98%E9%92%A5%E5%92%8C%E8%BF%9E%E6%8E%A5/%E5%BD%95%E5%B1%8F2025-12-19%2021.11.53.gif>)

配置文件开头添加如下配置

```yaml 
Host *
    IdentitiesOnly yes
    IdentityFile ~/.yourdotfiles/ssh/%n.pub
    ForwardAgent yes
```


![](https://assets.mitsea.cn/blog/posts/2025/12/%E4%BD%BF%E7%94%A8%20Bitwarden%20%E7%AE%A1%E7%90%86%20SSH%20%E7%A7%98%E9%92%A5%E5%92%8C%E8%BF%9E%E6%8E%A5/iShot_2025-12-19_21.19.07.avif)

IdentityFile 可以直接读与 Host 名称一致的公钥文件，这样可以简化配置

ForwardAgent 开启后，当你在服务器上执行需要 SSH 密钥的操作时，服务器会通过 SSH 通道回传请求，调用你本地电脑上的 Bitwarden SSH Agent。这样私钥永远不会离开你的本地机器，安全性最高，也不需要在服务器上残留可见的密钥文件。

> Photo by [Brooke Balentine](https://unsplash.com/@brookebalentine?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/photos/red-berries-with-water-droplets-on-a-branch-sv55rtLoDVY?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
      