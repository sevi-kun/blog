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

## Proxmox VE is awesome!

I've known [Proxmox VE](https://proxmox.com/en/proxmox-ve) now for some time, but I just recently started to use it extensivly. It is based on debian. Virtual machines are supportet through KVM. You can even run containers through LXC. It's all linux and all beautyful. Yet, I get why some peaple don't like it. Although the [documentation](https://pve.proxmox.com/pve-docs/) is awesome, you have to like the commandline and linux itself. You can't do anything via the gui. As soon as I understood this, I started to love it.

### Then, why this post?

You will find instructions in the documentations to passthrough GPUs, but these are outdated. In fact, the documentation should work when you're running version 6.1. But because in version 6.2 Proxmox step up to the 5.4 kernel, some modules have not to be loaded.

## PCIe passthrough

Now to the topic. My goal was to passthrough my GTX 1070ti to a Windows VM and set up gamestreaming via the nvidia shield API. 

## References
- [Allen Sampsell - Proxmox 6.1 and 6.2 PCIe Passthrough](https://youtu.be/_fkKIMF3HZw)
- [Chonklord - The Ultimate Beginner's Guide to GPU Passthrough (Proxmox, Windows 10)](https://www.reddit.com/r/homelab/comments/b5xpua/the_ultimate_beginners_guide_to_gpu_passthrough/)