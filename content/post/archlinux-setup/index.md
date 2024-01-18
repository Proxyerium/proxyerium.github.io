---
title: "Archlinux安装"
date: 2024-01-17T18:27:12+08:00
draft: true
---

Arch的安装确实照着官方教程做一遍就会了，不过每次安装总要看着wiki那还是挺麻烦的，把整个流程记下来方便日后再安装。

```shell
# iwctl联网
device list
station <device> scan
station <device> get-networks
station <device> connect <SSID>

# 硬盘分区
fdisk -l <disk>
fdisk <disk>
fdisk>> g # 新建GPT分区表
fdisk>> n # 新建分区
fdisk>> t # 更改分区类型 1 19 23
fdisk>> w

# 格式化分区
mkfs.fat -F 32 <EFI_partition>
mkswap <swap_partition>
mkfs.ext4 <root_partition>

# 挂载分区
mount <root_partition> /mnt
mount --mkdir <EFI_partition> /mnt/boot
swapon <swap_partition>

# 安装系统
vim /etc/pacman.d/mirrorlist # Server=https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch

pacstrap -K /mnt base linux linux-firmware amd-ucode networkmanager vim 

# 配置系统
genfstab -U /mnt >> /mnt/etc/fstab
arch-chroot /mnt
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
hwclock --systohc
vim /etc/locale.gen # en_US.UTF-8
locale-gen
vim /etc/locale.conf # LANG=en_US.UTF-8
echo <hostname> >> /etc/hostname
passwd

# bootloader
pacman -S grub efibootmanager
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB

# Nvidia
pacman -S nvidia nvidia-utils nvidia-setting

# 桌面环境
pacman -S gnome gnome-tweak
```
