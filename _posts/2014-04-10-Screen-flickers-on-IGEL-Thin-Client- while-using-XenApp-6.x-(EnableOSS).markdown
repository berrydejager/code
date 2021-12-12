---
layout: post
title: Screen flickers on IGEL Thin Client while using XenApp 6.x (EnableOSS)
date: 2014-04-10 13:37:00 +0100
description: Screen flickers on IGEL Thin Client while using XenApp 6.x (EnableOSS)
img: header/screen-flickers-on-igel-thin-client-while-using-XenApp-6_x-enableoos.png
tags: [PowerShell, PoSh, Scripting, Windows]
---
When you experience flickering of certain (in our case on blueish) parts of an internet website which being connected to a XenApp 6.x farm using a IGEL Thin Client you might need to read these Citrix articles :

[CTX133935 - Flickering or Other Display Issues Might be Seen when Adaptive Display or Progressive Display is Enabled](http://support.citrix.com/article/CTX133935) and [CTX128332 - Images are Tinted Blue in Published Photoshop](http://support.citrix.com/article/CTX128332)

This can easily be eliminated by setting the following setting in the setup of the Linux thin client:

    IGEL Setup -> System -> Registry -> ICA -> wfclient -> enableoss -> ◘ Enable off-screen drawing.

Unthicking this setting eliminates the problematic progressive / adaptive display feature when using OSS (Off-Screen Surfaces).

![](/assets/img/Screen-flickers-on-IGEL-Thin-Client- while-using-XenApp-6.x-(EnableOSS)_img00.png)
