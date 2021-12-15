---
layout: post
title: Unable to map printer due to PrintNightmare fix
date: 2021-06-17 13:37:00 +0100
description: Unable to map printer due to PrintNightmare fix
img: header/printnightmare.png
tags: [Windows, Windows 11, W11]
---
# PrintNightmare

PrintNightmare, a.k.a [CVE-2021-34527](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-34527), showed it that the Windows print spooler was vurnerable for remote code execution. 

Microsoft mitigated this by disallowing the non-admins to install printer drivers.

At a customer I experienced that the while updating the VDI image, which is not joined in the domain, could not install the drivers from the print server. 

The attempt resulted in the error 0x0000007c "Windows Cannot Connect to the Printer".

In the last few months, September and October 2021, Microsoft has been continuously releasing security updates that cause problems installing or printing to shared printers. Microsoft says it is working to resolve these issues, but once again many users are experiencing a lot of network printing problems after installing the [Windows 10 Cumulative Update 2021-10 (KB5006670)](https://support.microsoft.com/nl-nl/topic/12-oktober-2021-kb5006670-os-builds-19041-1288-19042-1288-en-19043-1288-8902fc49-af79-4b1a-99c4-f74ca886cd95).

In order to circumvent this a registy key was added temporarily.

    reg.exe add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Policies\Microsoft\FeatureManagement\Overrides /v 713073804 /t REG_DWORD /d 0 /f

After connecting the printers to retrieve the printer drivers for the new VDI image I removed the registry key again.

    reg.exe remove HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Policies\Microsoft\FeatureManagement\Overrides /v 713073804

The VDI image now contains the drivers and is still mitigated for the PrintNightmare bug.