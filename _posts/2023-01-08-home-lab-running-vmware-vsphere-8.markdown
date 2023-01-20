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

| Name | System | CPU(s) | RAM | Storage | Disabled NIC | Added NIC |
| --- | --- | --- | --- | --- | --- | --- |
| NAS00 | HP N40L Microserver G7 | ? | ? | ? | | |
| HOST00 | HP ML360-G6 | 2 x [Intel Xeon X5660 @ 2.8GHz](https://ark.intel.com/content/www/us/en/ark/products/47921/intel-xeon-processor-x5660-12m-cache-2-80-ghz-6-40-gts-intel-qpi.html) | 104 GB | 6 x 500GB = 2.5 TB RAID5 | HP NC326i (BCM5715) | HP NC110T |
| HOST01 | HP ML360-G6 | 2 x [Intel Xeon X5660 @ 2.8GHz](https://ark.intel.com/content/www/us/en/ark/products/47921/intel-xeon-processor-x5660-12m-cache-2-80-ghz-6-40-gts-intel-qpi.html) | 144 GB | ? | HP NC326i (BCM5715)   | HPE 331T (BCM5719) |
| HOST02 | HP ML360-G6 | 2 x [Intel Xeon X5660 @ 2.8GHz](https://ark.intel.com/content/www/us/en/ark/products/47921/intel-xeon-processor-x5660-12m-cache-2-80-ghz-6-40-gts-intel-qpi.html) | 144 GB | ? | HP NC326i (BCM5715)  | HPE 331T (BCM5719) |
| HOST03 | HP ML360-G6 | 2 x [Intel Xeon X5660 @ 2.8GHz](https://ark.intel.com/content/www/us/en/ark/products/47921/intel-xeon-processor-x5660-12m-cache-2-80-ghz-6-40-gts-intel-qpi.html) | 144 GB | ? | HP NC326i (BCM5715)  | HPE 331T (BCM5719) |

# Docker containers

The HP N40L acts as a Synology As the Xpenology, which runs the Synology software, 

| Docker container | Docker hub | 
homeassistant | [containrrr/watchtower](https://registry.hub.docker.com/r/containrrr/watchtower/)
ipmi-kvm | [lmacka/ipmi-kvm-docker](https://registry.hub.docker.com/r/lmacka/ipmi-kvm-docker/)
kms | [teddysun/kms](https://registry.hub.docker.com/r/teddysun/kms/)
unifi | [jacobalberty/unifi](https://registry.hub.docker.com/r/jacobalberty/unifi/)
watchtower | [containrrr/watchtower](https://registry.hub.docker.com/r/containrrr/watchtower/)
zt | [zerotier/zerotier-synology](https://registry.hub.docker.com/r/zerotier/zerotier-synology/)

# Unsupported hardware

NIC 


# Downloading the ISO

# Writing the ISO to USB

# Altering the USB

Add the following line to the `boot.cfg` on the USB stick

	AllowLegacyCPU=true

# Booting from USB

# Installing ESXi 8

# Disabling IPv6 support

'F2' 
