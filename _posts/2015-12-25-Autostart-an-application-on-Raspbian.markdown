---
layout: post
title: Autostart an application on Raspbian
date: 2015-12-25-13:37:00 +0100
description: Autostart an application on Raspbian
img: header/autostart-an-application-on-raspbian.png
tags: [Raspberry Pi, RPi, Raspbian]
---
If you want to autostart an application in Raspbian, just edit the following file;

    ~/.config/lxsession/LXDE-pi/autostart

contents of this file

    @lxpanel --profile LXDE-pi
    @pcmanfm --desktop --profile LXDE-pi
    @xscreensaver -no-splash
    @hexchat

I added ```@hexchat``` on the bottom to have HexChat IRC client autostarted at logon.