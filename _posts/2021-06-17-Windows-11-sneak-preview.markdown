---
layout: post
title: Windows 11 - sneak preview
date: 2021-06-17 13:37:00 +0100
description: Windows 11 sneak preview
img: header/windows_11_sneak_preview.png
tags: [Windows, Windows 11, W11]
---
# "Windows 10 is the last version of Windows...", ohh sure...

You might recall ~~Satya Nadella, CEO and chairman at Microsoft,~~ Jerry Nixon, developer evangelist at Microsoft, saying "Windows 10 is the last version of Windows". He didn't mention that it will be the final version of Windows that Microsoft will produce.

The next generation of Windows is about to be revealed at the [#MicrosoftEvent](https://www.microsoft.com/en-us/windows/event) on the 24th of June 2021 at 11:00  Eastern Time. Click [here](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWFdr0) for the reminder.

# My conclusion 

The verdict: As far as i can see it's a little more than just dressing up the Windows 10 interface visually. The interface of the operation system needs to compete with itself and other OS's to stay relevant in a designers world and at the same time it needs to be enticing for the end-user. 

[![](/assets/img/Window_11_sneak_preview_img10_t.jpg)](/assets/img/Window_11_sneak_preview_img10.png){:target="_blank"}

Having this said the VMware and Citrix OS optimalisation tools show some minor differences, see details below. To me it seems that the VMWare OSOT tool is more services focussed and Citrix Optimizer is more application oriented. Finding the right balance is just a matter of preference and trial and error.

## Brief history

Looking at the release cycle for the Windows Operation System, you will notice that the interval of the versions gradually increases. This has to do with the servicing updates and feature releases methodology that Microsoft embraces. Windows 10 Home and Pro have a retirement date set to 14th October 2025. As for the other variants we can only guess.

 The support lifecycle date of Windows 10 listed a 10-year support lifecycle.

| Year | Release |
| --- | --- | 
| 1981 | MS-DOS |
| 1985 | Windows 1.0 |
| 1987 | Windows 2.0 |
| 1990 | Windows 3.0 |
| 1992 | Windows 3.1 |
| 1993 | Windows NT 3.1 4.0|
| 1995 | Windows 95 |
| 1998 | Windows 98 |
| 2000 | Windows Me |
| 2000 | Windows 2000 |
| 2001 | Windows XP |
| 2006 | Windows Vista |
| 2009 | Windows 7 |
| 2012 | Windows 8 |
| 2015 | Windows 10 |
| 2021 | Windows 11 | 

## Installing Windows 11

The manual installing procedure is very similar to the Windows 10 procedure, however Windows 11 sports a more modern look, shows the OS-type date modified at "30th May 2021" and the license is updated in "June 2021". It seems that this Windows 11 was already in the pipeline for some while. 

[![](/assets/img/Window_11_sneak_preview_img00_t.jpg)](/assets/img/Window_11_sneak_preview_img00.png){:target="_blank"} [![](/assets/img/Window_11_sneak_preview_img01_t.jpg)](/assets/img/Window_11_sneak_preview_img01.png){:target="_blank"}

### Limiting experience

The "limited experience" offering, which is great for experimenting with the OS, is now hidden under "sign-in options" during the installation phase.

[![](/assets/img/Window_11_sneak_preview_img04_t.jpg)](/assets/img/Window_11_sneak_preview_img04.png){:target="_blank"} [![](/assets/img/Window_11_sneak_preview_img05_t.jpg)](/assets/img/Window_11_sneak_preview_img05.png){:target="_blank"} [![](/assets/img/Window_11_sneak_preview_img06_t.jpg)](/assets/img/Window_11_sneak_preview_img06.png){:target="_blank"}

Even the "This might take a few minutes"-screen has been animatified. You can actually skip this part by CTRL-ALT-DEL key combo and start the taskmanager, this will start the explorer.exe and you'll know what happens...

[![](/assets/img/Window_11_sneak_preview_img07_t.jpg)](/assets/img/Window_11_sneak_preview_img07.png){:target="_blank"}

This always makes me think of the [Impossible Mission](https://youtu.be/ivHFP3dJAkM?t=18) on the C-64.

### Windows 11 Activation

You can still use your KMS server to activate the Windows 11 version, while using the Windows 10 KMS keys. This is very handy at the time but may change in the RTM version.

### Intune / Autopilot

Deploying using Intune / Autopilot also worked. Having that said; Windows 11 reported itself with an OS version `10.0.21996.1`. Is it that the Windows 10 has been skinned and the version number need to be updated, who knows?

## UI/UX changes

The version of Windows 11 (Dev) tested showed up as "Version Dev (OS Build 21996.1)" when starting up `winver.exe`. 

### Overal re-design

[![](/assets/img/Window_11_sneak_preview_img02_t.jpg)](/assets/img/Window_11_sneak_preview_img02.png){:target="_blank"}

A brand new logo, by unskewing the Windows 10 logo to a square format, gets my eyes a little watery; Is this the real life? Is this just fantasy? It certainly looks more contemporary and elegant to me.

Shiny news icons, rounded corners, thin sliders and new animations are part of the design overhaul. 

Anyone got a handkerchief? 

[![](/assets/img/Window_11_sneak_preview_img03_t.jpg)](/assets/img/Window_11_sneak_preview_img03.png){:target="_blank"}

The OOBE also sports a new look. Not that is that important in a VDI world, `unattended.xml` and Autopilot are our friends. Most likely the ADK and unattend.xml will not change that much.
### Taskbar / Start menu

The most prominent change to the user interface is the taskbar/start menu. This is now "Maccyfied" into the center of the screen.

[![](/assets/img/Window_11_sneak_preview_img08_t.jpg)](/assets/img/Window_11_sneak_preview_img08.png){:target="_blank"}

For altering the taskbar settings you need to activate the Windows installation, see section "Windows 11 Activation" above.

The taskbar/start menu is still able to move to it's old place by setting the "Taskbar alignment" from `center` to `left`. Actually this whole taskbar menu has been changed quite a bit.

[![](/assets/img/Window_11_sneak_preview_img09_t.jpg)](/assets/img/Window_11_sneak_preview_img09.png){:target="_blank"}

Besides the taskbar the start menu also has changed.

When the changes of the start menu can't seem to fit in your workflow you can still revert to the Windows 10 menu style (a.k.a. [Classic mode](/assets/img/Window_11_sneak_preview_img11.png){:target="_blank"}. A little registry tweak will help you out.

`HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced`

| Name | Type | Data |
| --- | --- | --- |
| Start_ShowClassicMode | REG_DWORD | 0x00000001 (1) |

As this setting resides in the current user context of the Explorer UI, you need to log-off and log-on again to see the changes. Restarting the `explorer.exe` will also do the trick.

## Windows OS optimisation using industry standard tooling

### VMware OSOT

The [VMware OS Optimization Tool](https://flings.vmware.com/vmware-os-optimization-tool) helps in preparing and optimizing Windows 10 and Windows Server 2019, 2016 systems for use with VMware Horizon. For Windows 7, 8.1, and Server 2012, 2012 R2, an older version (b1130) of the OS Optimization Tool is available for download.

At a high level, the process of creating a golden image VM consists of a few steps. A a step by step walk-through of the complete process, is given in the [Creating an Optimized Windows Image for a VMware Horizon Virtual Desktop](https://techzone.vmware.com/creating-optimized-windows-image-vmware-horizon-virtual-desktop) guide.

#### Applying OSOT

Version used: [VMware OSOT, version B2003 - 20th April 2021](https://techzone.vmware.com/resource/vmware-operating-system-optimization-tool-guide#update) 

For applying OSOT to Windows 11 you need a specific Windows 11 template, which isn't available now/yet. Therefor I just selected the current 'Windows 10 and Server 2016 or later (v1.8)' template, published by VMware, available from the interface.

#### Comparing OSOT results: Windows 10 (21H1) against Windows 11 (Dev)

While comparing the OSOT analysis reports from an updated Windows 10 (21H1) against the Windows 11 Dev release, I noticed some differences.

| Setting in OSOT | Expected result | Windows 10 (21H1) | Windows 11 (Dev) |
| --- | --- | --- | --- | 
| Turn off hard disk after | 1 | N.A | 2 | 
| Disable Setup Cleanup Task | Disabled | Disabled | Enabled |
| Mobile Broadband Accounts - MNO Metadata Parser | Disabled | Enabled | Disabled |
| Background Intelligent Transfer Service | Disabled | Manual , Stopped | Auto , Running |
| Diagnostic Service Host | Disabled | Manual , Running | Manual , Stopped |
| Diagnostic System Host | Disabled | Manual , Running | Manual , Stopped |
| Payments and NFC/SE Manager Service | Disabled | Manual , Running | Manual , Stopped |
| Program Compatibility Assistant Service | Disabled | Manual , Running | Auto , Running |
| SSDP Discovery | Disabled | Manual , Stopped | Manual , Running |
| Show search icon in taskbar - Current User. | 1 | N.A | 1 |

### Citrix Optimizer

[Citrix optimizer](https://www.citrix.com/blogs/2020/10/13/tm-everything-you-wanted-to-know-about-citrix-optimizer-part-1/) optimizes user environments for better performance. It runs a quick scan of user environments and then applies template-based optimization recommendations. 

#### Applying Citrix Optimizer 

Version used: [Citrix Optimizer - v2.8.0.143](https://support.citrix.com/article/CTX224676)

Also for applying Citrix Optimizer to Windows 11 you need a specific Windows 11 template, which isn't available now/yet. Therefor I selected the current 'Windows 10 version 20H2 (2009) from Citrix" from the Citrix Optimizers' interface.

One of the Citrix Optimisations is removing "Microsoft.DesktopAppInstaller" built-in app. This didn't go very well Windows 11

    Microsoft.DesktopAppInstaller   Not Optimized
    Details: Failed to remove UWP app with following error: Removal failed. Please contact your software vendor.

    Module type: UWP

    - Name: Microsoft.DesktopAppInstaller

#### Comparing the Citrix Optimizer results: Windows 10 (21H1) against Windows 11 (Dev)

| Setting in Citrix Optimizer | Windows 10 (21H1) | Windows 11 (Dev) |
| --- | --- | --- |
| Microsoft.Microsoft3DViewer | Not Optimized, UWP app is installed | Optimized, UWP app is not installed |
| Microsoft.MixedReality.Portal | Not Optimized, UWP app is installed | Optimized, UWP app is not installed |
| Microsoft.Office.OneNote | Not Optimized, UWP app is installed | Optimized, UWP app is not installed |
| Microsoft.SkypeApp | Not Optimized, UWP app is installed | Optimized, UWP app is not installed |
| Microsoft.Wallet  | Not Optimized, UWP app is installed | Optimized, UWP app is not installed |
| Microsoft.XboxApp | Not Optimized, UWP app is installed | Optimized, UWP app is not installed |
| Mobile Broadband Accounts | Not Optimized, Scheduled Task is not in Disabled state | Optimized, Scheduled task does not exist |

## Details of used Windows 11 .ISO file

| Name | Description |
| --- | --- | 
| Name | 21996.1.210529-1541.co_release_CLIENT_CONSUMER_x64FRE_en-us.iso |
| Size | 4874553344 bytes (4648 MiB) |
| CRC32 | 48B651E0 |
| CRC64 | 92A6B5F8D18A819D |
| SHA256 | B8426650C24A765C24083597A1EBA48D9164802BD273B678C4FEFE2A6DA60DCB |
| SHA1 | 3B6DA9194BA303AC7DBBF2E521716C809500919C |
| BLAKE2sp | 8BBEAC022C2B41EFC05EAE24AA465B6AABB61B80FBA6624CA83FD5EEA48F1A15 |
