---
layout: post
title: Getting Citrix licensings statistics
date: 2015-04-16 13:37:00 +0100
description: Getting Citrix licensings statistics
img: header/getting-the-citrix-license-statistics.gif
tags: [PowerShell, PoSh, Scripting, Windows, Citrix, Citrix License server]
---
While being on the hunt for the daily statistics, I created a PowerShell-script that runs as a schedule job to harvest usage of the Citrix platform.

    [string]$result = (Get-Content 'c:\temp\CTX\CTX_licenses.txt')[4 .. 4]
    [string]$result_date = Get-Date
    $result_inuse = $result.substring(11,6)
    $result_users = $result.substring(31,6)
    $result_devices = $result.substring(47,6)
    $result_string = $result_date+";"+$result_inuse+";"+$result_users+";"+$result_devices
    Add-Content -path 'c:\temp\CTX\CTX_licenses.txt' -Value $result_string