+++
author = "FlintyLemming"
title = "HC620 HM-SMR 加密盘上手"
slug = "33c779ab646880199b07f46af06b1d1f"
date = "2026-04-08"
description = "btrfs 似乎也有问题"
categories = ["HomeLab"]
tags = ["NAS"]
image = "https://assets.mitsea.cn/blog/posts/2026/04/HC620%20HM-SMR%20%E5%8A%A0%E5%AF%86%E7%9B%98%E4%B8%8A%E6%89%8B/alexander-korte-Oj0ykWKf5To-unsplash.avif"
+++

因为最近 HDD 价格过于昂贵，所以这个盘比较火。我因为确实有一个专门备份的 Archive NAS，所以想试试这个盘是否堪用。

硬盘信息如下：

- **型号**: HGST Ultrastar DC HC620
- **容量**: 14TB
- **类型**: HM-SMR (Host-Managed Shingled Magnetic Recording)，4Kn，硬件级 SED 加密
- **Zone 大小**: 256 MiB，共 52156 个 Zone，最大 128 个 Open Zone

系统信息如下：

- **OS**: Ubuntu Resolute Raccoon (development branch) x86_64
- **Kernel**: Linux 7.0.0-12-generic
- **CPU**: Intel(R) Celeron(R) J1900 (4) @ 2.42 GHz
- **Memory**: 16 GiB

## 简单测试

硬盘的 SMART 没有问题。这个盘要注意 Helium_Level，据说低于临界值 25 就会无法写入。氦气一旦泄露就会持续下降，所以务必要选择 100 的硬盘。

![](https://assets.mitsea.cn/blog/posts/2026/04/HC620%20HM-SMR%20%E5%8A%A0%E5%AF%86%E7%9B%98%E4%B8%8A%E6%89%8B/image.avif)

此外这个盘进行快扫也没有发现问题。

## 问题：btrfs zoned mode 事务提交失败

因为这个盘比较特殊，普通 SMART 正常也不是很放心，所以我进行了连续几小时的写入测试。但是就在这个过程中发生了问题。

HM-SMR 盘在内核中被识别为 Zoned Block Device，btrfs 会自动启用 zoned mode。使用过程中出现以下错误链：

```
BTRFS error (device sda): error while writing out transaction: -11
BTRFS: Transaction aborted (error -11)        # -11 = EAGAIN
BTRFS info (device sda state EA): forced readonly
cow_file_range failed: -30                     # -30 = EROFS
```

后续写入全部失败

**根因：**trfs zoned mode 在事务提交时，zone 激活失败（活跃 zone 限制、write pointer 冲突等），返回 `-EAGAIN`，导致事务中止，文件系统被迫进入只读保护状态。
AI 认为这是 btrfs zoned mode 的软件层面 bug，非硬件故障。内核 7.0.0-12 对 zoned btrfs 支持不够成熟，上游虽有修复但未完全解决。

我比较同意，因为发生问题后，SMART 的 UDMA_CRC_Error_Count 并没有增长，还是 0

## 替代方案：dm-zoned + ext4

HM-SMR 盘无法直接使用 ext4/xfs 等常规文件系统（内核要求 zoned 感知），可选方案有限：

| 方案 | 成熟度 | 说明 |
| --- | --- | --- |
| **dm-zoned + ext4** | **高** | 通过 device mapper 将 zoned 设备模拟为常规块设备，上层使用成熟的 ext4 |
| btrfs zoned | 中 | 原生 zoned 支持，但有事务提交 bug，需频繁升级内核 |
| f2fs zoned | 低 | 支持 zoned，但大规模 HM-SMR 上验证不足 |
| zonefs | 低级 | 仅暴露 zone 为文件，非通用文件系统 |

既然 btrfs 已经出现问题，所以我换用了 **dm-zoned + ext4，**ext4 在大容量盘上久经考验，不需要担心 zoned 相关的事务 bug。经过连续 5 小时的写入测试，没有再发生任何异常。

不过缺点显而易见，就是我失去了 btrfs 的高级特性，cow 和 灵活的子卷快照。没有 cow 的话，在满盘时我可能会遇到显著的性能下降；没有快照对我影响倒不是很大，因为这个机器的数据不打算再往别的地方备份一次。我的备份策略是直接在来源端就往两个地方备份。

## 当前配置

```
/dev/sda (HM-SMR)
  └─ dm-zoned (/dev/mapper/dmz-VEGV73AY)
       └─ ext4 → /home/flintylemming/data
```

- 开机自动启动：`dm-zoned-start.service` 先激活 dm 设备，再由 fstab 挂载
- fstab 条目：`/dev/disk/by-id/dm-name-dmz-VEGV73AY /home/flintylemming/data ext4 noatime 0 2`

> Photo by [Alexander Korte](https://unsplash.com/@stelath?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/photos/airplane-wing-over-clouds-at-sunset-Oj0ykWKf5To?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
      