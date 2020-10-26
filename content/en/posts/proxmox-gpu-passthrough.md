---
title: "Proxmox GPU Passthrough"
author: "Béla Richartz"
date: 2020-10-24T11:35:07+02:00
description: "In this post I will describe how I managed to GPU Passthroug in Proxmox VE 6.2."
draft: true
image: images/feature4/proxmox-logo.png
authorEmoji: 🤖
tags:
- Proxmox
- GPU
- PCIe Passthroug
- Nvidia
- KVM
categories:
- linux
- virtualization
series:
- Proxmox VE
enableToc: true
enableTocContent: true
---

## Credits

Before we begin I would like to thank the Youtuber [Allen Sampsell](https://www.youtube.com/channel/UCRJp4NW0iHygU8sTsjFeE4g) and the reddit user [u/cjalas](https://www.reddit.com/user/cjalas/) which this post is mostly based on. They did a great job in explaining how and why. Without them I wouldn't be able to pass through my gpu and of course I would not have able to write this post.

## Proxmox VE is awesome!

I've known [Proxmox VE](https://proxmox.com/en/proxmox-ve) now for some time, but I just recently started to use it extensivly. It is based on debian. Virtual machines are supportet through KVM and you can run containers through LXC. tI's all linux and all beautyful. Yet, I get why some people don't like proxmox. Although the [documentation](https://pve.proxmox.com/pve-docs/) is awesome, you have to like the commandline and linux itself. You can't do anything in the gui. I hope, as soon as you overcome this, you will start to like proxmox as well.

### Then, why this post?

You will find instructions in the [documentation](https://pve.proxmox.com/pve-docs/) to passthrough GPUs, but in my case, these were outdated. There is a great [guide on reddit](https://www.reddit.com/r/homelab/comments/b5xpua/the_ultimate_beginners_guide_to_gpu_passthrough/) by [u/cjalas](https://www.reddit.com/user/cjalas/), but this doesn't work in the version 6.2. 

This is acctually the first post I write for this blog, so I don't want to overdo it. If there are any questions, please write a comment or feel free to send me a mail. 

## Notice

Before we begin, we should make sure your hardware supports the technologies needed. I wrote this guide for version 6.2. It may not work in later versions..
> "Your hardware should, at the very least, support: VT-d, interrupt mapping, and UEFI BIOS" </p>
> — <cite>u/cjalas[^1]</cite>

[^1]: The above quote is excerpted from the reddit post "[The Ultimate Beginner's Guide to GPU Passthrough (Proxmox, Windows 10)](https://www.reddit.com/r/homelab/comments/b5xpua/the_ultimate_beginners_guide_to_gpu_passthrough/)" which is written by [u/cjalas](https://www.reddit.com/user/cjalas/) on 27. March 2019.

### Hardware I use

#### Lenovo ThinkStation P900

- 2x Xeon E5-2630v3
- 2x 16GB 2666Mhz ECC RAM
- 4x 256Gb SATA SSD (later used in ZFS)
- 1x 126Gb SATA SSD (for Proxmox)
- Asus GTX 1070ti




## Configuring Proxmox

In this post I'm assuming that you have installed Proxmox 6.2 and configured your BIOS.

### 1. Configure Grub

First, we have to add something to the grub command line settings. For that, open /etc/default/grub:

```
nano /etc/default/grub
```

Search for a a line that looks like:

> GRUB_CMDLINE_LINUX_DEFAULT="quiet"

Replace it with:

```
GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on iommu=pt pcie_acs_override=downstream,multifunction vfio_iommu_type1.allow_unsafe_interrupts=1 nofb nomodeset video=vesafb:off,efifb:off"
```

or for amd:

```
GRUB_CMDLINE_LINUX_DEFAULT="quiet amd_iommu=on iommu=pt pcie_acs_override=downstream,multifunction vfio_iommu_type1.allow_unsafe_interrupts=1 nofb nomodeset video=vesafb:off,efifb:off"
```

After adding these settings, we have to update grub:

```
update-grub
```

### 2. Add VFIO modules

Again, edit the file /etc/modules with whatever text editor you like.

```
nano /etc/modules
```

Add the following lines to the file:

```
vfio
vfio_pci
vfio_virqfd
```

Save and exit.

### 3. KVM

If you use a Nvidia card you may want to set the following options:

```
echo "options kvm ignore_msrs=1" > /etc/modprobe.d/kvm.conf
```

For more information read [NVIDIA Tips](https://pve.proxmox.com/wiki/Pci_passthrough#NVIDIA_Tips) in the proxmox wiki.

### 4. Blacklist drivers

It would be realy bad if our proxmox host would try to use our GPU, so we need to blacklist the drivers.

Run these commands in the shell:

```
echo "blacklist radeon" >> /etc/modprobe.d/blacklist.conf
echo "blacklist nouveau" >> /etc/modprobe.d/blacklist.conf
echo "blacklist nvidia" >> /etc/modprobe.d/blacklist.conf
```

### 5.

## References
- [Allen Sampsell - Proxmox 6.1 and 6.2 PCIe Passthrough](https://youtu.be/_fkKIMF3HZw)
- [Chonklord - The Ultimate Beginner's Guide to GPU Passthrough (Proxmox, Windows 10)](https://www.reddit.com/r/homelab/comments/b5xpua/the_ultimate_beginners_guide_to_gpu_passthrough/)