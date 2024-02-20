---
title: "Archlinux安装"
date: 2024-01-17T18:27:12+08:00
slug: archlinux-setup
categories: [notes]
tags: [linux]
---

Arch的安装确实照着官方教程做一遍就会了，不过每次安装总要看着wiki挺麻烦的，把整个流程记下来方便以後要安装的时候。

****

## 基本

### iwctl联网

```shell
device list
station <device> scan
station <device> get-networks
station <device> connect <SSID>
```

### 硬盘分区

```shell
fdisk -l <disk>
fdisk <disk>
fdisk>> g # 新建GPT分区表
fdisk>> n # 新建分区
fdisk>> t # 更改分区类型 1 19 23
fdisk>> w
```

### 格式化分区

```shell
mkfs.fat -F 32 <EFI_partition>
mkswap <swap_partition>
mkfs.ext4 <root_partition>
```

### 挂载分区

```shell
mount <root_partition> /mnt
mount --mkdir <EFI_partition> /mnt/boot
swapon <swap_partition>
```

### 安装系统

```shell
vim /etc/pacman.d/mirrorlist # Server=https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch
pacstrap -K /mnt base linux linux-firmware amd-ucode networkmanager vim 
```

### 配置系统

```shell
genfstab -U /mnt >> /mnt/etc/fstab
arch-chroot /mnt
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
hwclock --systohc
vim /etc/locale.gen # en_US.UTF-8
locale-gen
vim /etc/locale.conf # LANG=en_US.UTF-8
echo <hostname> >> /etc/hostname
passwd
```

### bootloader

```shell
pacman -S grub efibootmanager
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
```

### Nvidia ~~fxxk you~~

```shell
pacman -S nvidia nvidia-utils nvidia-setting
```

### 桌面环境

```shell
pacman -S gnome gnome-tweak
```

## 双系统

> 先安装Windows，后安装Arch，同一个硬盘中的两个系统共用EFI分区。

在[挂载分区](#挂载分区)这一步中，`EFI_partition`是Windows安装时划分的：
```shell
mount <EFI_partition> /mnt/boot
```

在配置[bootloader](#bootloader)这一步中：
```shell
pacman -S grub efibootmanager os-prober
vim /etc/default/grub # GRUB_DISABLE_OS_PROBER=false
grub-mkconfig
```

## 参考资料

[Archlinux安装指南](https://wiki.archlinux.org/title/Installation_guide)

[Arch-Win双系统](https://wiki.archlinux.org/title/Dual_boot_with_Windows)
