---
layout: post
title: Simplistic way of getting memory statistics
date: 2014-10-31 13:37:00 +0100
description: Simplistic way of getting memory statistics
img: header/simplistic-way-of-getting-memory-statistics.gif
tags: [PowerShell, PoSh, Scriptint, Windows]
---

I came up with a simplistic way to harvest statistics from a Windows machine, using PowerShell;

    # $Author: Berry $
    # $Rev: 1.0 $
    #
    # Version control:
    #
    # 1.0 Initial version

    #
    # Functions definitions
    #

    Function GetFreeMem
    {
    Param([string]$ComputerName)
    Write-host “————————————————————————————”
    Write-host “- START SCANNING $Computername —————————————————”
    Write-host “————————————————————————————”
    Get-WmiObject -Class Win32_OperatingSystem -Namespace root/cimv2 -ComputerName . | Format-Table -Property TotalVirtualMemorySize,TotalVisibleMemorySize,FreePhysicalMemory,FreeVirtualMemory,FreeSpaceInPagingFiles
    Write-host “————————————————————————————”
    Write-host “- END SCANNING $Computername —————————————————–”
    Write-host “————————————————————————————”
    Write-Host ” ”
    Write-Host ” ”
    }
    ###
    ### Start of Main Routine
    ###

    clearGetFreeMem SERVER001
    GetFreeMem SERVER002
    GetFreeMem SERVER003

    ###
    ### End of Main Routine
    ###

