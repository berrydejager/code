---
layout: post
title: Redirect .OST from local to network location by using a symbolic link.
date: 2017-12-04 13:37:00 +0100
description: Redirect .OST from local to network location by using a symbolic link.
img: header/redirecting-the-.ost-file-to-a-network-share.gif
tags: [PowerShell, PoSh, Scripting, Windows, Outlook, .OST]
---

Using a VDI or SBC (Server Based Computing i.e. Citrix XenApp) recreating a new .OST file, for Microsoft Outlook, can be cumbersome. Redirecting the .OST file to a network share, which isn't supported by Microsoft, can relieve the machine from building a new .OST file for Outlook on every logon.

For this i created a PowerShell script achieve this.

```
#
# $Author: Berry $
# $Rev: 1.0 $
#
# Version control:
# 1.0 Redirect .OST from local to network location by using a symbolic link.
#
# Functionality:
# Rotating the log file
# Redirection of the OST location
#
#
# Start of function definitions
#
[string]$OSTFolder = $env:LOCALAPPDATA+'\Microsoft\Outlook'
[string]$OSTNetworkFolder = '\\\OST$\'
[string]$OSTNetworkFolderPersonal = $OSTNetworkFolder+$env:username
[string]$SystemDrive = Get-Content ENV:SystemDrive
[string]$LogFolder = 'Logfiles\'
[string]$LogPath = $OSTNetworkFolderPersonal + '\'
[string]$LogFile = “_Redirect_OST_log.txt”
[string]$LogFileDate = get-date -format “yyyy-MM-dd”
[string]$LogFileName = $LogFileDate + $LogFile

Function LogAction
{
	# Take parameter, string value, as action to log.
	Param([string]$a)
	# Determine actual date/time
	[string]$LogDate = Get-Date -format “dd-MM-yyyy HH:mm:ss.ms”
	# Write the actual date/time and parameter to the log file
	Add-Content -path $LogPath$LogFolder$LogFilename -value “$LogDate – $a”
}

Function LogRotate
{
	# Take parameter, an integer value, as days to keep
	param([int]$a)
	# Define variable $DaysToKeep as integer value
	If ($a -eq "")
		{[int]$DaysToKeep = 90}
	Else
		{[int]$DaysToKeep = $a}
	$LogFolderAvailable = Test-Path $LogPath$Logfolder
	If ($LogFolderAvailable -eq $false)
		{
		New-Item -Path $LogPath -Name $LogFolder -ItemType "directory"
		}
	# Check for logfiles older than <$DaysToKeep> days
	LogAction “--------------------------------”
	LogAction “Starting to rotate log files (retention set to $DaysToKeep days)”
	# Determine todays date minus $daysToKeep value
	$CompareDate = (Get-Date).AddDays(-$DaysToKeep)
	# Harvest a list of log files which are older than $DaysToKeep days
	$OldLogs= (Get-ChildItem $LogPath$LogFolder\*$Logfile) | Where-Object {$_.LastWriteTime -lt $CompareDate}
	$NumOldLogs=$OldLogs.Count
	$OldLogs | Remove-Item -Force
	LogAction "Number of log files to rotate: $NumOldLogs"
	LogAction ”Finished to rotating log files”
	LogAction “--------------------------------”
}

Function RedirectOST
{
	# Write logging information
	LogAction “* Starting Redirecting OST file *”
	$CurrentUserName = Get-Content ENV:UserName
	$FolderAvailable = Test-Path $OSTFolder
	If ($FolderAvailable -eq $False)
	{
		LogAction "Folder not found: $OSTFolder"
		LogAction "Creating new ReparsePoint for $OSTFolder directing to $OSTNetworkfolderPersonal"
		New-Item -Path $OSTFolder -ItemType SymbolicLink -Value $OSTNetworkFolderPersonal
		LogAction "Exiting script"
	}
	Else
	{
		$CheckReparsePoint = (Get-Item $OSTFolder | Where-object {$_.Attributes -match 'ReparsePoint'}).Name
		If ($CheckReparsePoint -eq 'outlook')
		{
			LogAction "ReparsePoint found: $OSTFolder"
			LogAction "Exiting script"
			Break
		}
		Else
		{
			LogAction "Removing folder $OSTFolder"
			Remove-Item -Path $OSTFolder -Recurse -Force
			LogAction "Creating new ReparsePoint for $OSTFolder directing to $OSTNetworkfolderPersonal"
			New-Item -Path $OSTFolder -ItemType SymbolicLink -Value $OSTNetworkFolderPersonal
			LogAction "Exiting script"
		}
	}
}

#
# End of function definitions
#
#
# Start of main routine
#
# Do house keeping on the logfiles by removing files older than $DaysToKeep
# When omitting the parameter the function defaults to 90 days to keep

LogRotate 7 #DaysToKeep

# Starting the RedirectOST function.

RedirectOST

#
# End of main routine
#
```