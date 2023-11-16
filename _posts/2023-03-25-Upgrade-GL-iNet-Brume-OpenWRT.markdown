---
layout: post
title: Upgrading GL.iNet Brume / GL-MV1000(W) to version 4.2.1
date: 2023-03-25 13:37:00 +0100
description: Upgrade your GL.iNet Brume to firmware v4.2.1 
img: header/upgrade-gl-inet-brume-v4_2_1.gif
tags: [GL-iNet, Brume, MV1000, MV1000W, Firmware, OpenWRT, Microsoft, Teams]
---

# Microsoft Teams disconnects the tethered network... Why?

The tethering connection on the GL.iNet Brume router is consistently dropped after a short period of time when using Microsoft Teams, often at inconvenient moments. I have determined that this behavior is not due to coincidence but rather a direct result of using Teams. Swapping out machines and operating systems didn't help. Also changing over to the web Teams client wasn't a workaround to the problem.

Proactively investigate the potential of upgrading the router software is the first course of action.

OpenWRT is a popular open-source operating system for routers and other networking devices. It provides a customizable and flexible platform for configuring and managing network settings, as well as advanced features such as VPN support, firewall rules, and bandwidth monitoring.

Like any software, OpenWRT is constantly evolving and improving. Developers release new versions of the operating system regularly to fix bugs, improve performance, and add new features. When you encounter issues with your router, upgrading to a newer version of OpenWRT can often help to resolve the problem.

Yes, updating the router software to a newer version of OpenWRT was the solution to the problem . In general, updating the software or firmware of your router is a common troubleshooting step to fix issues and improve overall performance. It's important to keep the router software up-to-date with the latest patches, bug fixes, and security updates to ensure optimal functionality and security of your network.

In my case the router is end-of-life so we need to be a little more creative, read below the details.

# The hardware

As far as the marketing concerns;

    __Edge Computing Gateway__

    Brume (GL-MV1000) and Brume-W (GL-MV1000W)* are powerful and stable networking products designed to do cutting-edge computing. With the Marvell high-performance chipset, the Brume and Brume-W* can run state-of-the-art cryptography at impressive speeds for an excellent VPN routing experience. Pre-installed OpenWrt and supported Ubuntu, Brume and Brume-W* allows in-depth developments for commercial IoT projects.

    Brume-W (GL-MV1000W)* is the special edition of Brume (GL-MV1000) that comes with an embedded Wi-Fi module, which delivers high-speed WiFi performance on 2.4GHz Wi-Fi with up to 300Mbps Wi-Fi speed.

# Updating...

It is always a good practice to stay up to date with firmware and software updates. In my situation, there was an additional motivation for me to look for a newer firmware.

The newer [Brume2 GL-MT2500 of GL.iNet](https://www.gl-inet.com/products/gl-mt2500/) routers sports OpenWRT 21.02 software while my current router runs OpenWRT 19.07.x software.

Let's try to update the firmware to a newer version, totally ignoring the router's end-of-life status and support level. If updating the firmware doesn't work, I'll [order the sleek Brume 2 / GL-MT2500](https://store.gl-inet.com/collections/brume-2-gl-mt2500-mt2500a-security-gateway).

While utilizing Microsoft Teams at my 'tiny office', the internet connection would frequently drop out. As there was no direct internet connectivity available at the office, I opted to purchase a compact and versatile router, specifically the [GL.iNet GL-MV1000](https://www.gl-inet.com/products/gl-mv1000/), and tethered it to my smartphone. Initially, this solution appeared to work well, but over time the internet connection began to drop out intermittently, possibly due to having Teams active and its updates. Eventually, the situation worsened to the point where it became too frustrating for me to tolerate. Diving deeper into the symptoms a solutions was found.

## The culprit

After spending some time investigating the issue by going through logs, analyzing network dumps, and searching the internet, I came across a post on the OpenWRT forum titled ['MS-TEAMS breaks NCM in trunk. Debugging help needed'](https://forum.openwrt.org/t/ms-teams-breaks-ncm-in-trunk-debugging-help-needed/77724/2). This post caught my attention as it described the same behavior that I was experiencing.

Microsoft Teams was causing my portable router to drop the tethering connection consistently after a short period of time. Even prior to troubleshooting this issue, I found that using Teams was not a very enjoyable experience for me, to put it mildly.

---

# Finding the firmware

While browsing for the [OpenWRT site](https://openwrt.org/toh/gl.inet/gl-mv1000) for alternative firmwares and attempting to install [the options provided](https://openwrt.org/toh/gl.inet/gl-mv1000#installation) I discovered that non of them could update the router from [3.125](https://dl.gl-inet.com/?model=mv1000) to newer 4.2.1.

However, when I switched to the [snapshot type of firmware](https://dl.gl-inet.com/?model=mv1000&type=snapshot), to my surprise, the updated firmware version (4.2.1) was available.

Download the 4.2.1 firmware for the Brume GL-MV1000 / GL-MV1000W from here:

*   [GL.iNet](https://fw.gl-inet.com/firmware/snapshots/20230324/mv1000/openwrt-mv1000-4.2.1-0324-1679603991.img)
*   [local mirror](/assets/bin/gl-inet/mv1000/4.x/openwrt-mv1000-4.2.1-0324-1679603991.img)

This file is for [local upgrade](https://docs.gl-inet.com/en/4/tutorials/firmware_upgrade/) in web Admin Panel and [Uboot](https://docs.gl-inet.com/en/4/tutorials/debrick/).

# Firmware compatibility

[GL-iNet CL-MV1000(W) - Brume(-W)](https://www.gl-inet.com/products/gl-mv1000/)

| Filename | Type | OpenWRT | 2.4G WiFi | GoodCloud.xyz | Remarks |
| --- | --- | --- | --- | --- | --- |
| [3.105](/assets/bin/gl-inet/mv1000/3.x/openwrt-mv1000-emmc-3.105.img) | Stable | 19.07.0-rc1 | Enabled | Online | Doesn't connect to tethered device |
| [3.115-0921](/assets/bin/gl-inet/mv1000/3.x/openwrt-mv1000-emmc-3.215-0921-1663732383.img) | Stable | - | -  | - | - |
| [3.216-0321](/assets/bin/gl-inet/mv1000/3.x/openwrt-mv1000-emmc-3.216-0321-1679391509.img) | Stable | 19.07.8 | Enabled | Online | Affected by the Teams-bug |
| [4.2.1-0324](/assets/bin/gl-inet/mv1000/4.x/openwrt-mv1000-4.2.1-0324-1679603991.img) | Snapshot |22.03.2 | Disabled | Online, no details in client overview | Has ZeroTier integration |
| [4.2.2-0504](/assets/bin/gl-inet/mv1000/4.x/openwrt-mv1000-4.2.2-0504-1683176079.img) | Snapshot | 22.03.x | Disabled | Online | - |
| [4.2.2-0518](/assets/bin/gl-inet/mv1000/4.x/openwrt-mv1000-4.2.2-0518-1684356632.img) | Snapshot | 22.03.x | Disabled | Online | - |
| [4.3.6-0801](/assets/bin/gl-inet/mv1000/4.x/openwrt-mv1000-4.3.6-0801-1690836333.img) | Snapshot | 22.03.x | Disabled | Online | - |
| [4.3.7-1018](/assets/bin/gl-inet/mv1000/4.x/openwrt-mv1000-4.3.7-1018-1697574734.img) | Snapshot | 22.03.x | Disabled | Online | - |
| [4.3.7-1107](/assets/bin/gl-inet/mv1000/4.x/openwrt-mv1000-4.3.7-1107-1699302665.img) | Snapshot | 22.03.x | Disabled | Online | - |
| [4.3.8-0927](/assets/bin/gl-inet/mv1000/4.x/openwrt-mv1000-4.3.8-0927-1695812269.img) | Beta | 22.03.4 | Disabled | Online | - |

# Updating the firmware

Although the updater offered the option to keep my current configuration after the update, it did not work as intended in my case. As a result, I had to manually configure the settings in the new firmware.

However, I discovered that creating a backup of the original 4.2.1 installation allowed me to modify the configuration files and then easily import the backup, saving me the hassle of having to repeatedly navigate through the graphical user interface.

__Bob's your uncle!__