+++
author = "FlintyLemming"
title = "Proxmox VE 8.1 vGPU 配置 （A6000）"
slug = "d29bb28b14984443b232263348b946ba"
date = "2023-12-13"
description = "新到的大玩具"
categories = ["Consumer", "Linux"]
tags = ["pve", "Nvidia"]
image = "https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/12/Proxmox%20VE%208.1%20vGPU%20%E9%85%8D%E7%BD%AE%20%EF%BC%88A6000%EF%BC%89/jigar-panchal-TVyPnkS5k5w-unsplash.avif"
+++

## 操作环境

Dell R750xa 配置如下

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/12/Proxmox%20VE%208.1%20vGPU%20%E9%85%8D%E7%BD%AE%20%EF%BC%88A6000%EF%BC%89/Untitled.avif)

## 设备配置

确保开启虚拟化和 SR-IOV

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/12/Proxmox%20VE%208.1%20vGPU%20%E9%85%8D%E7%BD%AE%20%EF%BC%88A6000%EF%BC%89/Untitled%201.avif)

## Proxmox VM host 环境配置

### 配置软件源

1. 删除企业源和 Ceph 源

    ```bash
    rm /etc/apt/sources.list.d/pve-enterprise.list
    rm /etc/apt/sources.list.d/ceph.list
    ```

2. 修改软件源为国内源

    ```bash
    nano /etc/apt/sources.list
    # 内容修改为如下内容
    deb https://mirrors.aliyun.com/debian/ bookworm main contrib non-free
    deb-src https://mirrors.aliyun.com/debian/ bookworm main contrib non-free
    deb https://mirrors.aliyun.com/debian/ bookworm-updates main contrib non-free
    deb-src https://mirrors.aliyun.com/debian/ bookworm-updates main contrib non-free
    deb https://mirrors.aliyun.com/debian/ bookworm-backports main contrib non-free
    deb-src https://mirrors.aliyun.com/debian/ bookworm-backports main contrib non-free
    deb https://mirrors.ustc.edu.cn/debian-security/ stable-security main contrib non-free
    deb-src https://mirrors.ustc.edu.cn/debian-security/ stable-security main contrib non-free
    ```

### 其他系统配置

1. 开启 iommu

    ```bash
    nano /etc/default/grub
    # 找到
    GRUB_CMDLINE_LINUX_DEFAULT="quiet"
    # 改为：
    GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on iommu=pt"
    # 更新 grub
    update-grub
    ```

2. 加载 vfio 模块

    ```bash
    echo vfio >> /etc/modules
    echo vfio_iommu_type1 >> /etc/modules
    echo vfio_pci >> /etc/modules
    echo vfio_virqfd >> /etc/modules
    ```

3. 屏蔽现有开源驱动，然后重启

    ```bash
    echo "blacklist nouveau" >> /etc/modprobe.d/blacklist.conf
    echo "blacklist nvidia" >> /etc/modprobe.d/blacklist.conf
    echo "blacklist nvidiafb" >> /etc/modprobe.d/blacklist.conf
    # 更新内核参数
    update-initramfs -k all -u
    ```

### 修改显卡模式

1. 如果 GPU 带显示接口，需要修改显卡模式。使用下面的命令检查，如果结果中显示为 VGA compatible controller 就需要修改。

    ```bash
    lspci | grep NVIDIA
    # 执行结果
    17:00.0 VGA compatible controller: NVIDIA Corporation GA102GL [RTX A6000] (rev a1)
    17:00.1 Audio device: NVIDIA Corporation GA102 High Definition Audio Controller (rev a1)
    65:00.0 VGA compatible controller: NVIDIA Corporation GA102GL [RTX A6000] (rev a1)
    65:00.1 Audio device: NVIDIA Corporation GA102 High Definition Audio Controller (rev a1)
    ca:00.0 VGA compatible controller: NVIDIA Corporation GA102GL [RTX A6000] (rev a1)
    ca:00.1 Audio device: NVIDIA Corporation GA102 High Definition Audio Controller (rev a1)
    e3:00.0 VGA compatible controller: NVIDIA Corporation GA102GL [RTX A6000] (rev a1)
    e3:00.1 Audio device: NVIDIA Corporation GA102 High Definition Audio Controller (rev a1)
    ```

2. 下载 NVIDIA Display Mode Selector Utility，可以从[这里](https://index.mitsea.com/%E8%BD%AF%E4%BB%B6/%E5%BA%94%E7%94%A8%E7%A8%8B%E5%BA%8F/Display_Mode-1.61.0.zip)下但是不保证链接有效性
3. 检查当前显卡，获得序号

    ```bash
    chmod +x displaymodeselector
    ./displaymodeselector --list
    ```

4. 修改显卡模式

    ```bash
    ./displaymodeselector --gpumode physical_display_disabled -i 0
    ./displaymodeselector --gpumode physical_display_disabled -i 1
    ./displaymodeselector --gpumode physical_display_disabled -i 2
    ./displaymodeselector --gpumode physical_display_disabled -i 3
    ```

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/12/Proxmox%20VE%208.1%20vGPU%20%E9%85%8D%E7%BD%AE%20%EF%BC%88A6000%EF%BC%89/Untitled%202.avif)

5. 重启服务器，重启后应该显示为 3D Controller

    ```bash
    17:00.0 3D controller: NVIDIA Corporation GA102GL [RTX A6000] (rev a1)
    65:00.0 3D controller: NVIDIA Corporation GA102GL [RTX A6000] (rev a1)
    ca:00.0 3D controller: NVIDIA Corporation GA102GL [RTX A6000] (rev a1)
    e3:00.0 3D controller: NVIDIA Corporation GA102GL [RTX A6000] (rev a1)
    ```

### 安装驱动

1. 安装 NVIDIA Driver 安装时需要的依赖

    ```bash
    apt update
    apt install build-essential dkms mdevctl pve-headers-$(uname -r)
    ```

2. 安装驱动，下载的驱动包有好几个驱动，安装 host 驱动。驱动可以从[这里](https://index.mitsea.com/%E8%BD%AF%E4%BB%B6/%E9%A9%B1%E5%8A%A8%E5%92%8C%E5%85%B6%E4%BB%96%E9%95%9C%E5%83%8F/NVIDIA-GRID-Linux-KVM-535.104.06-535.104.05-537.13.zip)下，但是不保证链接有效性。把驱动传到服务器上后，设置执行权限后运行。

    ```bash
    chmod +x NVIDIA-Linux-x86_64-535.104.06-vgpu-kvm.run
    ./NVIDIA-Linux-x86_64-535.104.06-vgpu-kvm.run --dkms
    ```

3. 执行 `nvidia-smi` 后无误即可

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/12/Proxmox%20VE%208.1%20vGPU%20%E9%85%8D%E7%BD%AE%20%EF%BC%88A6000%EF%BC%89/Untitled%203.avif)

## 搭建 vGPU 授权服务器

[Oscar Krause / FastAPI-DLS · GitLab](https://git.collinwebdesigns.de/oscar.krause/fastapi-dls)

按照仓库 Readme 搭建就行了，主要就是强制 https，本地的话需要生成一个自签名证书。法外狂徒挂公网可以无视，nginx 证书配好就行。对于挂在公网上有几个注意点：

1. docker 命令中的 `DLS_URL=`hostname -i`` 填你反代时要使用的域名例如`DLS_URL=`xxx.xxx.com`` 
2. `DLS_PORT=443` 不要动，只改 port 映射出去的端口，比如改成 `-p 4433:443` 这样反代那边就反代容器 IP:4433

## 虚拟机添加设备

开机后需要启用 SR-IOV 设备，每次开机都要执行，可以写成一个服务开机自动执行一次

```jsx
/usr/lib/nvidia/sriov-manage -e ALL
```

Raw Device 选择一个不是 .0 的设备后，MDev Type 就可以选 vGPU Profile 了。如果想要用整张显卡，也不要通 .0 的设备，据说会容易导致 pve 爆炸失联，建议还是选择一个用完所有显存的 Profile。

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/12/Proxmox%20VE%208.1%20vGPU%20%E9%85%8D%E7%BD%AE%20%EF%BC%88A6000%EF%BC%89/Untitled%204.avif)

![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/12/Proxmox%20VE%208.1%20vGPU%20%E9%85%8D%E7%BD%AE%20%EF%BC%88A6000%EF%BC%89/Untitled%205.avif)

## 激活 vGPU 授权

参考激活服务器 Readme 中 Setup Client 一节

[Oscar Krause / FastAPI-DLS · GitLab](https://git.collinwebdesigns.de/oscar.krause/fastapi-dls#setup-client)

### Windows

1. 进入 Windows 后先安装之前那个驱动包里的 host 驱动
2. 从 https://<你的dls服务器>/-/client-token 上下载配置文件，然后放到 C:\Program Files\NVIDIA Corporation\vGPU Licensing\ClientConfigToken 下
3. 重启电脑，然后就能看到正在获取许可证并激活成功

    ![](https://hf-index.mitsea.com:8840/d/Share/mitsea-public-source/blog/posts/2023/12/Proxmox%20VE%208.1%20vGPU%20%E9%85%8D%E7%BD%AE%20%EF%BC%88A6000%EF%BC%89/CleanShot_2023-12-13_at_22.17.132x.avif)

### Linux

执行下面的命令

```bash
curl --insecure -L -X GET https://<dls-hostname-or-ip>/-/client-token -o /etc/nvidia/ClientConfigToken/client_configuration_token_$(date '+%d-%m-%Y-%H-%M-%S').tok
service nvidia-gridd restart
```

> Photo by [Jigar Panchal](https://unsplash.com/@brave4_heart?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/a-very-colorful-abstract-background-with-a-lot-of-blocks-TVyPnkS5k5w?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)
  