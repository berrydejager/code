---
layout: post
title: Getting the keyboard layout using PowerShell
date: 2013-01-10 13:37:00 +0100
description: Getting the keyboard layout using PowerShell
img: header/getting-the-keyboard-layout.gif
tags: [PowerShell, PoSh, Scripting, Windows]
---
Just a little PowerShell script to grab the keyboard layout(s) of the current user;

	[string]$HiveKey = “HKCU:\Keyboard Layout\Preload”
	$Values=(Get-Item $HiveKey).GetValueNames() | Sort -Descending
	ForEach ($Value in $Values)
	{
	Write-Host SetLocale (Get-ItemProperty $HiveKey).($value)
	}

