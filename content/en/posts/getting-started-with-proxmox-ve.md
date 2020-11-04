---
title: "Getting started with Proxmox VE"
author: "Béla Richartz"
date: 2020-10-24T11:35:07+02:00
description: "In this post I wrote how I configure Proxmox. It should be a help to anyone who want's to set up a homelab with proxmox."
draft: true
image: images/feature4/proxmox-logo.png
authorEmoji: 🤖
tags:
- Proxmox
- KVM
categories:
- linux
- virtualization
series:
- Proxmox VE
enableToc: true
enableTocContent: true
---
These are some things, you may need to know:
- If you are going to passthroug a PCI device, you must use UEFI boot. I also deactivated CSM, so I don't have any trouble with it. A drawbag of this whole thing is that you  this post I will describe how I managed to GPU Passthroug in Proxmox VE 6.2. 

