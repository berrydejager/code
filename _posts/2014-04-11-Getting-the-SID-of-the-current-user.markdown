---
layout: post
title: Getting the SID of the current user
date: 2014-04-11 13:37:00 +0100
description: Getting the SID of the current user
img: header/getting-the-sid-of-the-current-user.gif
tags: [PowerShell, PoSh, Scripting, Windows]
---
Getting the SID of the current user, using PowerShell, is remarkably simple. This one-liner retrieves the SID of the current logged on user.

    Get-WmiObject win32_useraccount -Filter "name='$env:username' and domain='$env:userdomain'"
