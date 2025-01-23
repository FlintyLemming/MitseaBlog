+++
author = "FlintyLemming"
title = "【翻译】管理配置文件最好的方法：一个 bare git 仓库"
slug = "4b7df285915b4fc19d2d6af4e3817b84"
date = "2021-04-22"
description = ""
categories = ["Linux"]
tags = ["Windows", "Linux"]
image = "https://image.mitsea.com/blog/posts/2021/04/%E3%80%90%E7%BF%BB%E8%AF%91%E3%80%91%E7%AE%A1%E7%90%86%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E6%9C%80%E5%A5%BD%E7%9A%84%E6%96%B9%E6%B3%95%EF%BC%9A%E4%B8%80%E4%B8%AA%20bare%20git%20%E4%BB%93%E5%BA%93/title.avif"
+++

原文地址：[https://www.atlassian.com/git/tutorials/dotfiles](https://www.atlassian.com/git/tutorials/dotfiles)

原标题：The best way to store your dotfiles: A bare Git repository

译者注：本文的 dotfiles 原意指的是一些隐藏的目录或者文件，但由于实际需要管理的一般是配置文件。所以在翻译中我就把 dotfile 引申为 配置文件。

声明：标题有点绝对，肯定也有解决该问题的其他好方法。我只是介绍下我认为比较好的方案。

最近我在 [Hacker News](https://news.ycombinator.com/item?id=11070797) 上看了很多人关于管理他们[配置文件](https://en.wikipedia.org/wiki/Hidden_file_and_hidden_directory)的方法。用户 `StreakyCobra` 所[展示的方法](https://news.ycombinator.com/item?id=11071754)非常棒，我也打算像他一样管理配置文件。而且条件很简单，我只要事先安装好 [Git](https://www.atlassian.com/git) 即可。

对于这套方案，按照他的原话说就是：

没有用到任何额外的工具，也没有用到 symlinks，只是用到了版本管理工具。你可以为不同的电脑创建不同的分支，也可以在新电脑上直接获得已经保存的配置。

这个方案特别创建了用于保存配置文件的目录，并且在执行命令中均指定清楚了目录地址，所以不会对你其他的 git 操作产生影响。

## 从头开始

如果你还没有用 Git 仓库管理你的配置文件，你可以用下面的命令快速管理起来。

```bash
git init --bare $HOME/.cfg
alias config='/usr/bin/git --git-dir=$HOME/.cfg/ --work-tree=$HOME'
config config --local status.showUntrackedFiles no
echo "alias config='/usr/bin/git --git-dir=$HOME/.cfg/ --work-tree=$HOME'" >> $HOME/.bashrc
```

- 第一行会创建一个 `~/.cfg` 文件夹，[Git bare 仓库](https://www.saintsjd.com/2011/01/what-is-a-bare-git-repository/) 会把这个文件夹给管理起来。
- 然后创建一个 alias，这句话的意思就是当我们在 shell 中执行 `config`，就等于执行 `/usr/bin/git --git-dir=$HOME/.cfg/ --work-tree=$HOME` ，方便特别管理这个 .cfg 文件夹。
- 因为是手动指定 `$HOME` 里的文件并上传，所以我们将 `status.showUntrackedFiles` 关闭，这样在查看仓库状态的时候，不会因为其他文件导致显示还有文件没有提交。
- 由于 alias 是 shell 命令，不能持久化，所以要想重启后还能继续使用。需要把这个 alias 命令写到你现在用的 shell 的配置文件里。（原文里用的是 bash）

然后你就可以用 `config` 命令去添加、管理你的配置文件了。比如你要把 vim 和 bash 的配置文件都加到 Git 里，就像这样执行命令即可。

```bash
config status
config add .vimrc 
config commit -m "Add vimrc"
config add .bashrc
config commit -m "Add bashrc"
config push
```

当然，在执行 config push 前，需要配置下远程仓库。为了照顾 Git 新手，我这边放一下我的操作步骤

```bash
config remote add origin https://github.com/FlintyLemming/FlintyConfig.git
config branch -M main
config push -u origin main
```

![](https://image.mitsea.com/blog/posts/2021/04/%E3%80%90%E7%BF%BB%E8%AF%91%E3%80%91%E7%AE%A1%E7%90%86%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E6%9C%80%E5%A5%BD%E7%9A%84%E6%96%B9%E6%B3%95%EF%BC%9A%E4%B8%80%E4%B8%AA%20bare%20git%20%E4%BB%93%E5%BA%93/1.avif)

### 需要确认的事情

1. 配置文件中已经包含了当前 shell 的配置文件，并且里面配置了刚才的 alias
2. 把 `.cfg` 添加到 `.gitignore` 里，这样在拉取时就不会导致递归问题
    
    ```bash
    echo ".cfg" >> .gitignore
    ```
    

## 在新环境上找回你的配置

如果你已经在云端仓库保存了你的配置文件，你可以按照下面的步骤取回来：

- 把仓库里的内容下载下来：
    
    ```bash
    git clone --bare <git-repo-url> $HOME/.cfg
    ```
    
- 设置 alias：
    
    ```bash
    alias config='/usr/bin/git --git-dir=$HOME/.cfg/ --work-tree=$HOME'
    ```
    
- checkout 云端的配置文件到你的 `$HOME` 目录下：
    
    ```bash
    config checkout
    ```
    
- 上面这步可能会报错，就像这样：
    
    ```bash
    error: The following untracked working tree files would be overwritten by checkout:
        .bashrc
        .gitignore
    Please move or remove them before you can switch branches.
    Aborting
    ```
    
    这是因为你的 `$HOME` 目录已经有这些文件了，发生了冲突。解决方法很简单：如果你不需要原来的文件，删掉即可；如果需要，就备份一下。这边提供一个冲突文件自动备份到特定文件夹下的方法：
    
    ```bash
    mkdir -p .config-backup && \
    config checkout 2>&1 | egrep "\s+\." | awk {'print $1'} | \
    xargs -I{} mv {} .config-backup/{}
    ```
    
    然后重新 checkout
    
    ```bash
    config checkout
    ```
    
- 同样把 `status.showUntrackedFiles` 关闭：
    
    ```bash
    config config --local status.showUntrackedFiles no
    ```
    
- 大功告成，现在你可以像之前一样管理配置文件了，就像这样：
    
    ```bash
    config status
    config add .vimrc
    config commit -m "Add vimrc"
    config add .bashrc
    config commit -m "Add bashrc"
    config push
    ```
    

## 结语

希望这个方法能帮助你管理配置文件。我在这边也分享一下[我的配置文件仓库](https://bitbucket.org/durdn/cfg/src/master/)。别忘记关注我和我团队的 Twitter，[@durdn](https://www.twitter.com/durdn) 和 [@atlassiandev](https://www.twitter.com/atlassiandev)。

译者：如果觉得不错的话，可以去 [Medium](https://mitsea.medium.com/%E7%BF%BB%E8%AF%91-%E7%AE%A1%E7%90%86%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E6%9C%80%E5%A5%BD%E7%9A%84%E6%96%B9%E6%B3%95-%E4%B8%80%E4%B8%AA-bare-git-%E4%BB%93%E5%BA%93-d2be0da4729c) 给我点个 Clap，或者去 [Bilibili 专栏](https://www.bilibili.com/read/cv10982105) 投个币点个赞。