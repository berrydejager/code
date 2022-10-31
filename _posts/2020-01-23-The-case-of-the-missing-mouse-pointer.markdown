---
layout: post
title: The case of the missing mouse pointer
date: 2020-01-23 13:37:00 +0100
description: Suddenly, while working with multiple RDP sessions from my W10 VDI machine, the ‘normal select’ mouse pointer vanished.
img: header/the-case-of-the-missing-mouse-pointer.gif
tags: [Microsoft, Windows, RDP]
---
Suddenly, while working with multiple RDP sessions from my W10 VDI machine, the ‘normal select’ mouse pointer vanished. Remarkably the other cursors, like ‘text select’ and ‘link select’ ,would still show.

Very frustrating as the root of the problem remains unknow to me. While trying to resolve this i came up with a very easy fix.

Within the RDP execute the following sequence:

*	Start ‘main.cpl’
*	Navigate to ‘Pointers’ tab using the Tab-key
*	Press ALT+e to unthick the ‘Enable Pointer Shadow’ option
*	Press ALT+a to apply the settings

![The-case-of-the-missing-mouse-pointer_img00.gif](/assets/img/The-case-of-the-missing-mouse-pointer_img00.gif)

The mouse pointer has returned again.

Faith in Windows; restored... for now...

