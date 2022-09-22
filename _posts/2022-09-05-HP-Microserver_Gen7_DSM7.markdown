---
layout: post
title: Installing DSM7 on a HP Proliant MicroSserver Gen7
date: 2022-04-29 13:37:00 +0100
description: How to install DSM7 using TCRP on a HP Proliant MicroServer GEN7 / G7.
img: header/HPMS-G7_DSM7.png
tags: [XPEnology, DSM7, HP, MicroServer, Synology, TinyCore, RedPill]
---

# Why

# What to use?

https://github.com/pocopico/tinycore-redpill

# What not to use?

# How to configure

# tinycore-redpill
This is a testing version. Do not use unless you are certain you have no data to lose.

# Instructions 

A normal build process would start with :

1. Image burn img file to usb stick

2. Boot tinycore

3. SSH to your booted loader or just open the desktop terminal 

4. Bring over your json files; 
    * `./global_config.json`
    * `./custom_config.json`
    * `./user_config.json`

5. Check the contents of `user_config.json`, if satisfied keep or else run :

    a.  Perform a rploader update by running;
        `./rploader.sh update now`

    b.  Perform a fullupdate to update all local files of your image by running; 
        `./rploader.sh fullupgrade now`

    c.  Change you serial and mac address by running
        `./rploader.sh serialgen DS3615xs`
        if you want to use WoL you can use realmac option here e.g.;
        `./rploader.sh serialgen DS3615xs realmac`

    d. Update `user_config.json` with your `VID:PID` of your usb stick by running;
        `./rploader.sh identifyusb now`

    e. Update `user_config.json` with your SataPortMap and DiskIdxMap by running 
        `./rploader.sh satamap now`

    f. Backup your changes to local loader disk by running  
        `./rploader.sh backup now`

6. ./rploader.sh build bromolow-7.0.1-42218
