---
layout: post
title: Linux kernel 6.x vs. DisplayLink (EVDI)
date: 2022-10-28 13:37:00 +0100
description: Fixing the broken EVDI support for the DisplayLink drivers
img: header/linux-kernel-evdi.png
tags: [Linux, PopOS, DisplayLink, Kernel]
#published: false
---

Kernel 6.x broke the support for the EVDI which is shipped with current release (5.6.1) of the [DisplayLink drivers for Ubuntu](https://www.synaptics.com/products/displaylink-graphics/downloads/ubuntu). This is really annoying as all of a 'sudden' my workstations which sport [Pop!_OS 22.04 LTS](https://pop.system76.com/) wouldn't work with external monitors, connected using a DisplayLink device, anymore. This happened by an upgrade package overnight (around 25th of October 2022). It took me a little while to realize what the actual problem is.

# Culprit

When reinstalling the [Pop!_OS 22.04_14 ISO](https://iso.pop-os.org/22.04/amd64/intel/14/pop-os_22.04_amd64_intel_14.iso) again; no problem!

In the meantime a new version, [Pop!_OS 22.04_15](https://iso.pop-os.org/22.04/amd64/intel/15/pop-os_22.04_amd64_intel_15.iso) was released; again no additional monitors.

This made me vigurously looking what was going on. From the log files, it became clear to me that the kernel was bugging the compatibility with the EVDI drivers. 

So, I decided to look into that direction and stumbled upon [this question on GitHub](https://github.com/DisplayLink/evdi/issues/383), submitted by [BasSmeets](https://github.com/BasSmeets).

# Solution

The solution is the replaced the shipped evdi.tar.gz file, containing the EVDI driver sources, in the installed package of the downloaded installed file. This can be done by following a simple procedure as described by [Karly](https://www.displaylink.org/forum/member.php?u=23243) on the [DisplayLink forum](https://www.displaylink.org/forum/showpost.php?p=92453&postcount=3).

As I have installed the `displaylink-driver-5.6.1-59.184.run` file I will use this as the base line;

## Uninstall the current EVDI driver. 

    sudo ./displaylink-driver-5.6.1-59.184.run uninstall

## Extract the content of the installer file to a seperate folder.

    ./displaylink-driver-5.6.1-59.184.run --noexec --keep

## Enter the extracted folder

    cd ./displaylink-driver-5.6.1-59.184

## Replace the `evdi.tar.gz` file from the DisplayLink GitHub repository

    curl -L https://github.com/DisplayLink/evdi/archive/refs/heads/devel.tar.gz -o evdi.tar.gz

## Modify the `displaylink-installer.sh` file at line #38:

    if ! tar xf "$TARGZ" -C "$EVDI"; then

and replace it with:

    if ! tar xf "$TARGZ" -C "$EVDI" --strip-components=1; then

## Run the installer script as root

This installs and compiles the very latest EVDI driver with works with the current 6.0.2 kernel

    sudo ./displaylink-installer.sh  

## Enjoy your multi-monitor setup again.

![WHOOHOO](assets/img/linux-kernel-evdi-whoohoo.png)

# Gratitude

Thanks the following people:

*   [Karly](https://www.displaylink.org/forum/member.php?u=23243)
    *   For pointing out a solution
*   [BasSmeets](https://github.com/BasSmeets)
    *   For creating an issue on GitHub
*   [Wesly](https://github.com/wezzynl
    *   For thinking along with me.
