---
title: Linux篇-启动项配置
tags: 
    - linux
    - boot
categories: 
    - linux
thumbnail: /images/linux/boot-config/thumbnail.png
excerpt: linux启动项选择与基本配置
---

## Preface

After installing linux, we need to boot it. With any luck, you will get a brand new operating system that you can get right into and use. But it seems that we are not always the luckiest ones. Maybe what awaits us is a device with a black screen, and that doesn't seem to be what we want. So we need to avoid this, and under linux, a boot loader is always a must.

## Guide

### check firmware types

Firmware may sound confusing, and it's not. In short, the following two firmware systems are commonly used in modern computers:

> BIOS:Basic Input-Output System(has been replaced progressively since 2010 by the UEFI)
>
> UEFI:Unified Extensible Firmware Interface

### system initialization

The system turns on, performs a power-up self-test, and initializes the necessary hardware devices. After this, the base firmware scans the corresponding disk partition and finds the corresponding add-in, then starts the bootloader, after which the bootloader loads the kernel and the system using specific parameters.

### boot loader

The boot loader is the first software loaded by the computer firmware and is responsible for loading the kernel using specific kernel parameters and initializing the RAM disks according to the configuration file.

**some boot loader(from arch wiki)**

![bootloaders](../images/linux/boot-config/bootloader.png)

Here we can find a number of bootloaders to choose from, and the applicable characteristics of common bootloaders are also provided here, here we are going to use **grub** as it has the most powerful characteristics and is also one of the most popular bootloader programs.

### Grub

#### download and install 

```shell
#I'm using grub on arch, 
#if you're using other distributions,
# looking for specific downloads is necessary!
sudo pacman -S grub
grub-install --target=i386-pc /dev/sdX(Installed disks, not partitions)
```

#### config

```shell
grub-mkconfig -o /boot/grub/grub.cfg
```

## Summary

## Reference

[Arch Boot Process](https://wiki.archlinux.org/title/Arch_boot_process#Boot_loader)

[Grub from arch](https://wiki.archlinux.org/title/GRUB)