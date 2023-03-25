---
layout: post
title: Upgrading GL.iNet Brume / GL-MV1000(W) to version 4.2.1
date: 2023-03-25 13:37:00 +0100
description: Upgrade your GL.iNet Brume to firmware v4.2.1 
img: header/upgrade-gl-inet-brume-v4_2_1.gif
tags: [GL-iNet, Brume, MV1000, MV1000W, Firmware, OpenWRT, Microsoft, Teams]
---

# Why would you update?

It is always a good practice to stay up to date with firmware and software updates. In my situation, there was an additional motivation for me to look for a newer firmware.

The newer [Brume2 GL-MT2500 of GL.iNet](https://www.gl-inet.com/products/gl-mt2500/) routers sports OpenWRT 21.02 software while my current router runs OpenWRT 19.07.x software.

Let's try to update the firmware to a newer version, totally ignoring the router's end-of-life status and support level. If updating the firmware doesn't work, I'll [order the sleek Brume 2 / GL-MT2500](https://store.gl-inet.com/collections/brume-2-gl-mt2500-mt2500a-security-gateway).

While utilizing Microsoft Teams at my 'tiny office', the internet connection would frequently drop out. As there was no direct internet connectivity available at the office, I opted to purchase a compact and versatile router, specifically the [GL.iNet GL-MV1000](https://www.gl-inet.com/products/gl-mv1000/), and tethered it to my smartphone. Initially, this solution appeared to work well, but over time the internet connection began to drop out intermittently, possibly due to having Teams active and its updates. Eventually, the situation worsened to the point where it became too frustrating for me to tolerate. Diving deeper into the symptoms a solutions was found.

## The problem

The tethering connection on the GL.iNet Brume router is consistently dropped after a short period of time when using Microsoft Teams, often at inconvenient moments. I have determined that this behavior is not due to coincidence but rather a direct result of using Teams. Swapping out machines and operating systems didn't help. Also changing over to the web Temas client wasn't a workaround to the problem.

## The culprit

After spending some time investigating the issue by going through logs, analyzing network dumps, and searching the internet, I came across a post on the OpenWRT forum titled ['MS-TEAMS breaks NCM in trunk. Debugging help needed'](https://forum.openwrt.org/t/ms-teams-breaks-ncm-in-trunk-debugging-help-needed/77724/2). This post caught my attention as it described the same behavior that I was experiencing.

Microsoft Teams was causing my portable router to drop the tethering connection consistently after a short period of time. Even prior to troubleshooting this issue, I found that using Teams was not a very enjoyable experience for me, to put it mildly.

---

# Finding the firmware

While browsing for the [OpenWRT site](https://openwrt.org/toh/gl.inet/gl-mv1000) for alternative firmwares and attempting to install [the options provided](https://openwrt.org/toh/gl.inet/gl-mv1000#installation) I discovered that non of them could update the router from [3.125](https://dl.gl-inet.com/?model=mv1000) to newer 4.2.1.

However, when I switched to the [snapshot type of firmware](https://dl.gl-inet.com/?model=mv1000&type=snapshot), to my surprise, the updated firmware version (4.2.1) was available.

Download the 4.2.1 firmware for the Brume GL-MV1000 / GL-MV1000W from here:

*   [GL.iNet](https://fw.gl-inet.com/firmware/snapshots/20230324/mv1000/openwrt-mv1000-4.2.1-0324-1679603991.img)
*   [local mirror](/assets/bin/openwrt-mv1000-4.2.1-0324-1679603991.img)

This file is for [local upgrade](https://docs.gl-inet.com/en/4/tutorials/firmware_upgrade/) in web Admin Panel and [Uboot](https://docs.gl-inet.com/en/4/tutorials/debrick/).

Date Compiled: 2023-03-23 21:39:51 (UTC+01:00)
SHA256: `de1a774a072793f9447d158a68de8bcf447daa8fe3c369db6d6fb994dcc87bbd`  

# Updating the firmware

Although the updater offered the option to keep my current configuration after the update, it did not work as intended in my case. As a result, I had to manually configure the settings in the new firmware.

However, I discovered that creating a backup of the original 4.2.1 installation allowed me to modify the configuration files and then easily import the backup, saving me the hassle of having to repeatedly navigate through the graphical user interface.

__Bob's your uncle!__