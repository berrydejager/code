---
layout: post
title: Rebooting a Citrix Server
date: 2013-01-25 13:37:00 +0100
description:  
img: header/rebooting-a-citrix-server.gif
tags: [PowerShell, PoSh, Scripting, Windows, Citrix, Solution4, S4, Login Consultants]
---
Rebooting a Citrix Server, restarting using the default scheme in Citrix wasn't cutting it. So I created a script for it.

	#
	# $Author: Berry $
	# $Rev: 1.4 $
	#
	# Version control:
	#
	# 1.0 Initial version in powershell scripting
	# 1.1 Changed the log rotate to 90 days, added the print spooler task kill
	# 1.2 20-11-2012 Added Function GetCurrentDateTime to proper logfile entry creation
	# 1.3 03-01-2013 Completely restructured and re-written
	# 1.4 23-06-2013 Added Function SetMachineConfigRights and set email alert to: alert@bogus.tld

	#
	# Functions definitions
	#

	Function LogAction
	{
		Param([string]$a)
		[string]$LogDate = Get-Date -format “dd-MM-yyyy HH:mm:ss.ms”
		Add-Content -path $LogFileName -value “$LogDate – $a”
	}

	Function LogRotate
	{
		param([int]$a)
		# Check for logfiles older than <$a> days
		LogAction “—————————————————-”
		LogAction “Starting to rotate log files”
		[int]$DaysToKeep = $a
		$CompareDate = (Get-Date).AddDays(-$DaysToKeep)
		Get-ChildItem $LogFolder\*$LogFile | Where-Object {$_.LastWriteTime -lt $CompareDate} | Remove-Item
		LogAction ” Finished to rotating log files”
	}

	Function LogOffCitrixUsers
	{
		Add-PSSnapin Citrix* -ErrorAction SilentlyContinue
		[array]$CheckActiveSessions = Get-XASession -ServerName $CitrixServer | where {$_.Protocol -match “ICA” -and $_.AccountName -match “$UserDomain”}

		If ($CheckActiveSessions.length -gt 0){

			do {
				[array]$ActiveSessions = Get-XASession -ServerName $CitrixServer | where {$_.Protocol -match “ICA” -and $_.AccountName -match “$Userdomain”}
				for ($i=0; $i -lt $ActiveSessions.length ; $i++)
				{
					LogAction “Attempting to log off”
					LogAction $ActiveSessions[$i].AccountName
					LogAction $ActiveSessions[$i].SessionId
					LogOff $ActiveSessions[$i].SessionId
					LogAction “Sleeping for 8192 milliseconds.”
					Start-Sleep -Milliseconds 8192
				}
			}	while ($ActiveSessions.Count -gt 0 )
		}
		else
		{
			LogAction “No active user sessions found.”
		}
	}

	Function ApplyPatchesAndReboot
	{
		Param([string]$a)
		[string]$EmailAddress = $a
		[string]$StartExe = “c:\windows\syswow64\ApplyPatchesAndreboot.exe”
		[string]$StartExeArgs = “/noreboot /mailonfail:$EmailAddress”
		LogAction “Starting to apply patches”
		start-process $StartExe $StartExeArgs -Wait
		LogAction ” Finished applying Patches”
	}

	Function SetMachineConfigRights
	{
		LogAction “Starting SetMachineConfigRights”
		$FindMachineConfig = get-childitem -path C:\Windows\Microsoft.NET -Recurse -Filter “Machine.Config”
		$FindMachineConfig = $FindMachineConfig.Fullname
		For ($i=0; $i -lt $FindMachineConfig.length ; $i++)
		{
			LogAction ” Setting ACL for “$FindMachineConfig[$i]
			icacls $FindMachineConfig[$i] /grant “Domain Users:(M)”
			}
		LogAction ” Finished SetMachineConfigRights”
	}

	Function RemoveSpoolFiles
	{
		LogAction “Cleaning obsolete files from the spooler directory”
		$SpoolFolder = Get-ItemProperty HKLM:\SYSTEM\CurrentControlSet\Control\Print\Printers | select “DefaultSpoolDirectory”
		[int]$SpoolFileCount = 0
		Get-ChildItem -Path $SpoolFolder.DefaultSpoolDirectory | ForEach-Object {$SpoolFileCount++}
		If ($SpoolFileCount -gt 0)
		{
			LogAction ” Removing $SpoolFileCount spool files”
			Get-ChildItem $SpoolFolder.DefaultSpoolDirectory | foreach ($_) {remove-item $_.fullname}
		}
		Else
		{
			LogAction ” No obsolete spool files found”
		}
	}

	Function KillService {
		Param([string]$a, [string]$b) # the two parameters.
		[string]$ServiceName = $a
		[string]$ServiceDescription = $b
		[int]$n = 3 # second to wait
		LogAction “Attempting to abort $b”
		If (Get-Process $ServiceName -ErrorAction SilentlyContinue)
		{
			Stop-Process -processname $ServiceName -Force
			Start-Sleep -Seconds $n
			If (Get-Process $ServiceName -ErrorAction SilentlyContinue)
			{
				LogAction ” ***** FAILED *****”
			}
			Else
			{
				LogAction ” $ServiceDescription abortion confirmed”
			}
		}
		Else
		{
			LogAction ” * $ServiceDescription not running *”
		}

	Function StartService {
		Param([string]$a) # Service description.
		[string]$ServiceDescription =$a
		[int]$n = 3 # second to wait
		LogAction “Starting $a after $n seconds”
		Start-Sleep -Seconds $n
		Start-Service $a
		Get-Service “Print Spooler” | ForEach-Object { 
			if ($_.status -eq “running”)
			{LogAction ” Service – $a – running”}
			else
			{
				LogAction ” ***** Service -$a- not running *****”
			}
		}
	}

	Function RebootServer {
		LogAction ” ***** Initiating Reboot *****”
		. c:\s4\Framework\Core\S4-Preload.ps1;S4-PreLoad;S4\Invoke-Systemreboot
		LogAction ” ***** Reboot intiated *****”
	}

	###
	###
	### Start of Main Routine
	###
	###

	################## PREREQUISITES ##################
	################## PREREQUISITES ##################
	################## PREREQUISITES ##################

	[string]$SystemDrive = Get-Content ENV:SystemDrive
	[string]$SystemRoot = Get-Content ENV:SystemRoot
	[string]$CitrixServer = Get-Content ENV:COMPUTERNAME
	[string]$UserDomain = Get-Content ENV:USERDOMAIN
	[string]$LogFolder = $SystemDrive+”\Logs\”
	[string]$LogFile = “_Reboot_log.txt”
	[string]$LogFileDate = get-date -format “dd-MM-yyyy”
	[string]$LogFileName = $LogFolder + $LogFileDate + $LogFile

	### Do house keeping on the logfiles by rotating over $DaysToKeep

	LogRotate 90 #DaysToKeep

	### User log off

	LogOffCitrixUsers

	### Killing the Citrix Print Manager
	KillService cpsvc “Citrix Print Manager Service”

	### Kill Microsoft Print Spooler Service
	KillService spoolsv “Print Spooler”

	### Cleaning up the obsolote spool files from the spooler directory
	RemoveSpoolFiles

	### Starting Microsoft Print Spooler Service
	StartService “Print Spooler”

	### Starting Citrix Print Manager Service
	StartService “Citrix Print Manager Service”

	### Start process Apply Patches (without reboot!)
	### email the results on failure to address mentioned in first argument
	alert@bogus.tld

	### Change the ACL for the MACHINE.CONFIG files in the various Microsoft .NET folders
	SetMachineConfigRights

	### Starting Citrix Print Manager Service
	RebootServer

	###
	###
	### End of Main Routine
	###
	###

