---
title: "Proxmox without subscription"
author: "Béla Richartz"
description: "In this post I wrote how I configure Proxmox. It should be a help to anyone who want's to set up a homelab with proxmox."
draft: true
image: images/feature4/proxmox-logo.png
authorEmoji: 🤖
tags:
- Proxmox
- KVM
- setup
categories:
- linux
- virtualization
series:
- Proxmox VE
enableToc: true
enableTocContent: true
---

## Proxmox VE post installation

Hey. You just installed Proxmox VE and don't have a subscription? In this post I will show you how I set up my Proxmox.

This will be a pretty short post, because I only will show how I removed the Subscription message and how I set the testing (non-subscription) repositories. 

## Before start

- Proxmox VE 6.2
- Intel core

## Removing subscription message

1. Click on your Proxmox host. (For me this would be "pve") 
2. Open the Shell.
3. Navigate to "/usr/share/javascript/proxmox-widget-toolkit"

```bash
cd /usr/share/javascript/proxmox-widget-toolkit
```

4. Make a backup of the existing proxmoxlib.js

```bash
cp proxmoxlib.js proxmoxlib.js.bak
```

5. Now we can edit the file proxmoxlib.js I will use vim as my text editor.

```bash
vim proxmoxlib.js
```

6. Search for 'No valid subscription'

For me this is on 