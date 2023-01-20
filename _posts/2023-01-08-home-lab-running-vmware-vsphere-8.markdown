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

My homelab consists of a few old machines, cheapskated over the years. At first, VMware vSphere 6.5 was running on the stock hardware. However the need of progressing to vSphere 8.

## Servers

| Name | Stock hardware | CPU(s) | RAM | Storage | Disabled NIC | Added NIC |
| --- | --- | --- | --- | --- | --- | --- |
| NAS00 | HP N40L Microserver G7 | AMD Turion II Neo N40L @ 1.5 Ghz K625  | 8GB | 4 x 3.5" 2TB SATA, 2 x 2.5" 120GB SSD | | |
| HOST00 | HP ML360-G6 | 2 x [Intel Xeon X5660 @ 2.8GHz](https://ark.intel.com/content/www/us/en/ark/products/47921/intel-xeon-processor-x5660-12m-cache-2-80-ghz-6-40-gts-intel-qpi.html) | 104 GB | 6 x 3.5" 500GB SATA | HP NC326i (BCM5715) | HP NC110T |
| HOST01 | HP ML360-G6 | 2 x [Intel Xeon X5660 @ 2.8GHz](https://ark.intel.com/content/www/us/en/ark/products/47921/intel-xeon-processor-x5660-12m-cache-2-80-ghz-6-40-gts-intel-qpi.html) | 144 GB | 8 x 2.5" 147GB SAS | HP NC326i (BCM5715)   | HPE 331T (BCM5719) |
| HOST02 | HP ML360-G6 | 2 x [Intel Xeon X5660 @ 2.8GHz](https://ark.intel.com/content/www/us/en/ark/products/47921/intel-xeon-processor-x5660-12m-cache-2-80-ghz-6-40-gts-intel-qpi.html) | 144 GB | 6 x 2.5" 147GB SAS | HP NC326i (BCM5715)  | HPE 331T (BCM5719) |
| HOST03 | HP ML360-G6 | 2 x [Intel Xeon X5660 @ 2.8GHz](https://ark.intel.com/content/www/us/en/ark/products/47921/intel-xeon-processor-x5660-12m-cache-2-80-ghz-6-40-gts-intel-qpi.html) | 144 GB | 8 x 2.5" 147GB SAS | HP NC326i (BCM5715)  | HPE 331T (BCM5719) |

## Network
| Type | Device | Trivia |
| --- | --- | --- |
| Modem | iPhone 6s | Unlimited 4G data plan |
| Router | [GL.iNet Brume GL-MV1000](https://www.gl-inet.com/products/gl-mv1000/) | USB tethering to the router |
| Switch | [JGS524E](https://www.netgear.nl/support/product/JGS524E.aspx) | 


# Docker containers

The HP N40L acts as a DS3615xs Synology, running Xpenology and Synology DSM 6.2-23739

| Docker container | Docker hub | 
homeassistant | [containrrr/watchtower](https://registry.hub.docker.com/r/containrrr/watchtower/)
ipmi-kvm | [lmacka/ipmi-kvm-docker](https://registry.hub.docker.com/r/lmacka/ipmi-kvm-docker/)
kms | [teddysun/kms](https://registry.hub.docker.com/r/teddysun/kms/)
unifi | [jacobalberty/unifi](https://registry.hub.docker.com/r/jacobalberty/unifi/)
watchtower | [containrrr/watchtower](https://registry.hub.docker.com/r/containrrr/watchtower/)
zt | [zerotier/zerotier-synology](https://registry.hub.docker.com/r/zerotier/zerotier-synology/)

# Unsupported hardware

The onboard/embedded NIC of the ML350G6, which sports a BCM5715 chipset, is working in vSphere 6.x but support has been deprecated for vSphere 8. This means that another NIC is needed to physically connect the servers to the switch.

# Altering the USB

Add the following line to the `boot.cfg` on the USB stick

	AllowLegacyCPU=true

# Booting from USB

# Installing ESXi 8

# Disabling IPv6 support

'F2' 
