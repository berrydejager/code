---
layout: post
title: Running a cheapskate home lab VMware vSphere 8
date: 2023-01-08 13:37:00 +0100
description: Running a cheapskate home lab VMware vSphere 8
img: header/vmware-vsphere-unsupported-hardware.gif
tags: [VMware, vSphere, ESXi, vCenter]
published: false
---
# The homelab

My homelab consists of a few old machines I have cheapskated over the years.

| Name | System | CPUs | RAM | Storage | Trivia |
| --- | --- | --- | --- | --- | --- |
| NAS00 | HP NL40L Microserver G7 | ? | ? | ? | |
| HOST00 | HP ML360-G6 | ? | ? | ? | |
| HOST01 | HP ML360-G6 | ? | ? | ? | |
| HOST02 | HP ML360-G6 | ? | ? | ? | |
| HOST03 | HP ML360-G6 | ? | ? | ? | |

# Unsupported hardware

LSI 9211-8i

# Downloading the ISO

# Writing the ISO to USB

# Altering the USB

Add the following line to the `boot.cfg` on the USB stick

	AllowLegacyCPU=true

# Booting from USB

# Installing ESXi 8

# Disabling IPv6 support

'F2' 
