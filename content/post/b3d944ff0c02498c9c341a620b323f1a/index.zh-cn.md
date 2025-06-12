+++
author = "FlintyLemming"
title = "群晖 DS1621+ 添加 E10M20-T1 的支持"
slug = "b3d944ff0c02498c9c341a620b323f1a"
date = "2023-06-25"
description = "群晖你干的好啊.jpg"
categories = ["HomeLab", "Linux"]
tags = ["群晖", "设备树"]
image = "https://img.flinty.moe/blog/posts/2023/06/%E7%BE%A4%E6%99%96%20DS1621%2B%20%E6%B7%BB%E5%8A%A0%20E10M20-T1%20%E7%9A%84%E6%94%AF%E6%8C%81/johannes-mandle-r_FNlKOxxos-unsplash.avif"
+++

## 起因

实际上群晖官方并没有让 DS1621+ 支持 E10M20-T1，我买他主要是有两个原因：

1. 根据 Reddit 上用户的[提醒](https://www.reddit.com/r/synology/comments/ksaw7s/comment/gif30av/?utm_source=share&utm_medium=web2x&context=3)，相同 CPU 和 PCIe x4 的 RS1221+ 是支持这款扩展卡的。所以硬件上应该是没有障碍的。实际上卡到手后也证明了这一点，这就是个使用 ASM PCIe 交换芯片的 plx 卡，插在 PCIe x4 的 Windows 电脑上所有功能都可以正常使用。
2. https://github.com/007revad/Synology_HDD_db这个 GitHub 脚本 Readme 说该脚本可以 “Optionally enables M2D20, M2D18, M2D17 and E10M20-T1 on Synology NAS that don't officially support them.”

## 折腾历程

### 尝试脚本

既然硬件没限制，又有脚本号称可以支持，那我直接进行一个冲动消费，1575，卡就到手上了

![](https://img.flinty.moe/blog/posts/2023/06/%E7%BE%A4%E6%99%96%20DS1621%2B%20%E6%B7%BB%E5%8A%A0%20E10M20-T1%20%E7%9A%84%E6%94%AF%E6%8C%81/IMG_2186.avif)

结果是，脚本 log 认出了这张卡，也认到了卡上的 nvme SSD，但就是在 WebGUI 里看不到，无法管理，网卡也用不了。但我觉得硬件上肯定是没限制，只是这个卡没什么大冤种会买，用的人比较少罢了，所以作者的脚本没有适配全面。我就继续研究。

### 启用网卡

脚本解决不了问题第一时间是搜 issue，找到了这条讨论记录

[Unrecognized firmware DS1823xs+? · Issue #87 · 007revad/Synology_HDD_db](https://github.com/007revad/Synology_HDD_db/issues/87#issuecomment-1595386455)

根据描述 `/usr/syno/etc.defaults/adapter_cards.conf` 文件似乎控制着扩展卡的功能，发现关于这个扩展卡的有三段：`[E10M20-T1_sup_nic]`、`[E10M20-T1_sup_nvme]`和`[E10M20-T1_sup_sata]`。根据名称，应该就是限制这张卡的网卡、nvme SSD 支持和 ngff SSD 支持所适用的机型。

我就把这三个项目里都加上了 `DS1621+=yes` ，重启后 nvme SSD 还是识别不了，但是网卡识别了。至此，问题解决了一部分。

![](https://img.flinty.moe/blog/posts/2023/06/%E7%BE%A4%E6%99%96%20DS1621%2B%20%E6%B7%BB%E5%8A%A0%20E10M20-T1%20%E7%9A%84%E6%94%AF%E6%8C%81/%25E6%2588%25AA%25E5%25B1%258F2023-06-25_15.38.58.avif)

### 手动创建存储池？

虽然 WebGUI 里看不到硬盘，但是 /dev 下还是能看到的。考虑到之前 nvme 不能做存储池的时候，都是可以先格式化硬盘然后用 mdadm 命令手动创建一个存储池。具体创建方法一搜一大堆，我这边随便贴一个帖子，步骤其实都一样的

[[Guide] Use NVME SSD as storage volume instead of cache in DS918](https://www.reddit.com/r/synology/comments/a7o44l/guide_use_nvme_ssd_as_storage_volume_instead_of/)

然而并不行，比如我创建了个 md9 出来，但是重启后这个 md9 就消失了，存储管理器里也看不到任何存储池。

### extensionPorts？

我记得之前看过关于黑群晖启用 nvme 存储池的帖子，里面是要修改一个叫 extensionPorts 的文件。内容类似于这篇帖子的回复

[NVMe cache support](https://xpenology.com/forum/topic/13342-nvme-cache-support/page/9/#comment-308677)

然而并没用，我系统里这个文件里已经有了

![](https://img.flinty.moe/blog/posts/2023/06/%E7%BE%A4%E6%99%96%20DS1621%2B%20%E6%B7%BB%E5%8A%A0%20E10M20-T1%20%E7%9A%84%E6%94%AF%E6%8C%81/%25E6%2588%25AA%25E5%25B1%258F2023-06-25_15.47.55.avif)

### Device Tree

遇到难题后，我自己也提了个 issue

[Can not enable E10M20-T1 on DS1621+ · Issue #97 · 007revad/Synology_HDD_db](https://github.com/007revad/Synology_HDD_db/issues/97#issuecomment-1605868704)

出乎意料的是，[@007revad](https://github.com/007revad) 大佬真的非常认真，他帮忙比对了 1621+ 和 1221+ 设备的相关文件。他认为应该是设备树文件导致的差异，不过他不确定群晖是否校验 Device Tree Blob 文件，似乎也没有条件尝试。但他帮我从 1221+ 的 pat 文件里提取了 dtb 文件供我参考，真的超级好。我对 Linux 和 DSM 也就是略懂，所以我又去请教了 [Jim](https://github.com/jim3ma) 大佬，他说 DSM 的设备树文件是可以改的，不校验的。考虑到我的情况，他建议我直接看我机器的 dtb 文件。通过执行 `dtc -I dtb -O dts -o - /run/model.dtb`，我对比了两个设备的 dtb 文件发现主要就是缺少一段

```dts
E10M20-T1 {
		compatible = "Synology";
		model = "synology_e10m20-t1";

		m2_card@1 {

			nvme {
				pcie_postfix = "00.0,08.0,00.0";
				port_type = "ssdcache";
			};
		};

		m2_card@2 {

			nvme {
				pcie_postfix = "00.0,04.0,00.0";
				port_type = "ssdcache";
			};
		};
	};
```

但是如果添加到 1621+ 里的话，`pcie_postfix` 不知道写什么。这个是群晖自己创造的东西，不懂他的含义。不过考虑到现在扩展卡里的硬盘 PCIe 信息正好符合这个后缀规律，所以我觉得这里应该不用改，就应该是 `00.0,08.0,00.0`

![](https://img.flinty.moe/blog/posts/2023/06/%E7%BE%A4%E6%99%96%20DS1621%2B%20%E6%B7%BB%E5%8A%A0%20E10M20-T1%20%E7%9A%84%E6%94%AF%E6%8C%81/%25E6%2588%25AA%25E5%25B1%258F2023-06-25_16.04.28.avif)

下面开始修改。把 model.dtb 文件搞出来后，使用下面的命令反编译 dtb 文件为可编辑的 dts 文件

```bash
dtc -I dtb -O dts -o model.dts model.dtb
```

添加缺失的 E10M20-T1 支持

![](https://img.flinty.moe/blog/posts/2023/06/%E7%BE%A4%E6%99%96%20DS1621%2B%20%E6%B7%BB%E5%8A%A0%20E10M20-T1%20%E7%9A%84%E6%94%AF%E6%8C%81/%25E6%2588%25AA%25E5%25B1%258F2023-06-25_17.04.23.avif)

将修改好的 dts 文件重新编译为 dtb 文件

```bash
dtc -I dts -O dtb -o model.dtb model.dts
```

保存好源文件，替换掉 `/etc.defaults/model.dtb` 文件即可，`/run` 下面的同名文件不用管，`/run` 是个临时目录

## 修改效果

![](https://img.flinty.moe/blog/posts/2023/06/%E7%BE%A4%E6%99%96%20DS1621%2B%20%E6%B7%BB%E5%8A%A0%20E10M20-T1%20%E7%9A%84%E6%94%AF%E6%8C%81/%25E6%2588%25AA%25E5%25B1%258F2023-06-25_17.28.26.avif)

> Photo by [Johannes Mändle](https://unsplash.com/@leonardo_64?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
  
