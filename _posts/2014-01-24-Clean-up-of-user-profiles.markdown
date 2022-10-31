---
layout: post
title: 
date: 2014-01-24 13:37:00 +0100
description:  
img: header/clean-up-of-user-profiles.gif
tags: [PowerShell, PoSh, Scripting, Windows]
---


	#
	#    $Author: Berry $
	#    $Rev: 2.1 $
	#
	#    Version control:
	#    1.0 	Initial version in powershell scripting
	#    2.0	Complete rewritten and using REMPROF.EXE
	#    2.1	Added ‘REMPROF.EXE /AD:0’ to remove all abandoned registry and directory entries
	#
	#    Functionality:
	#    Rotating the log file
	#    Cleaning the C:\Users folder
	#    Cleaning the Profile list registry
	#

	[string]$SystemDrive = Get-Content ENV:SystemDrive
	[string]$LogFolder = $SystemDrive+”\Logfiles\”
	[string]$LogFile = “_Cleanup_log.txt”
	[string]$LogFileDate = get-date -format “dd-MM-yyyy”
	[string]$LogFileName = $LogFolder + $LogFileDate + $LogFile

	#
	# Start of function definitions
	#

	Function LogAction
	{
	#    Take parameter, string value, as action to log.
	Param([string]$a)
	#    Determine actual date/time
	[string]$LogDate = Get-Date -format “dd-MM-yyyy HH:mm:ss.ms”
	#    Write the actual date/time and parameter to the log file
	Add-Content -path $LogFileName -value “$LogDate – $a”
	}

	Function LogRotate
	{
	#    Take parameter, an integer value, as days to keep
	param([int]$a)
	#    Check for logfiles older than <$a> days
	LogAction “—————————————————-”
	LogAction “Starting to rotate log files”
	#    Define variable $DaysToKeep as integer value
	[int]$DaysToKeep = $a
	#    Determine todays date minus 90 $daysToKeep value
	$CompareDate = (Get-Date).AddDays(-$DaysToKeep)
	#    Harvest a list of log files which are older than $DaysToKeep days
	Get-ChildItem $LogFolder\*$LogFile | Where-Object {$_.LastWriteTime -lt $CompareDate} | Remove-Item
	LogAction ”   Finished to rotating log files”
	}

	Function CleanUpUserProfileDir
	{
	#    Write logging information
	LogAction “* Starting housekeeping on the filesystem *”
	$CurrentUserName = Get-Content ENV:UserName

	$query = Get-Childitem C:\Users
	foreach ($global:record in $query )
	{
	[Array]$records += $record.Name
	}

	[array]$Global:skip = “Administrator”, “All Users”, “AltirisAdmin”, “Ctx_StreamingSvc”, “Default”, “Default User”, “Public”, “TEMP”
	$skip += Get-Content ENV:USERNAME
	foreach ($record in $records)
	{    $record = $record.Split(“.”)[0]
	if ($skip -notcontains $record)
	{
	LogAction “Profile for $record will be deleted”
	C:\Tools\RemProf.exe `””$record`”” | find /i `””$record`”” | Out-File $LogFileName -Append -Encoding ASCII
	}
	else
	{
	LogAction “Profile for $record will be preserved”
	}
	}
	LogAction “Deleting all user profiles that have no username association including abandoned profile folders.”
	C:\Tools\RemProf.exe | Out-File $LogFileName -Append -Encoding ASCII
	LogAction “* Finished housekeeping on the filesystem *”
	}

	#
	# End of function definitions
	#

	#
	# Start of main routine
	#
	# Do house keeping on the logfiles by removing files older than $DaysToKeep
	LogRotate 90 #DaysToKeep

	CleanUpUserProfileDir

	#
	# End of main routine
	#


