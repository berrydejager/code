---
layout: post
title: Cleaning the Internet Explorer cache
date: 2013-05-23 13:37:00 +0100
description: Cleaning the Internet Explorer cache
img: header/clean-internet-esxplorer-cache.png
tags: [PowerShell, PoSh, Scripting, Windows, Printing, Printer]
---
A need for cleaning the Internet Explorer cache was risen. I took this as an opportunity to explore to do this using PowerShell and build a little script.

	#
	#	Name:		CleanInternetExplorerCache.ps1
	#
	#	Function:	Clean up the Internet Explorer Cache
	#
	#   $Author: 	Berry $
	#
	#   Version:	1.0	Initial version in powershell scripting
	#
	[string]$Version = "1.0"

	#	RunDll32.exe InetCpl.cpl,ClearMyTracksByProcess [#]
	#	1		Delete History
	#	2		Delete Cookies
	#	8		Delete Temporary Internet Files
	#	16		Delete Form Data
	#	32		Delete Stored Passwords
	#	255		Delete All
	#	4351	Delete All with the "Also delete files and settings stored by add-ons" options selected
	#
	#	Usage	http://technet.microsoft.com/en-us/library/ff730941.aspx
	#



	Function ShowHelp
	{
		Write-Host ""
		Write-Host "Usage:`tCleanInternetExplorerCache [option]"
		Write-Host ""
		Write-Host "Options:"
		Write-Host ""
		Write-Host "`tAllAddOns`t-`tClears all info"
		Write-Host "`tAllInfo`t`t-`tClears all information"
		Write-Host "`tCookies`t`t-`tClears the stored cookies"
		Write-Host "`tFiles`t`t-`tClears the files from the cache"
		Write-Host "`tFormData`t-`tClears the stored form data"
		Write-Host "`tHelp`t`t-`tShow this info"
		Write-Host "`tHistory`t`t-`tClears the Internet Explorer history"
		Write-Host "`tPasswords`t-`tClears stored passwords"
		Write-Host ""
		Write-Host "Example:"
		Write-Host ""
		Write-Host "`tCleanInternetExplorerCache Files"
		Write-Host ""
		Write-Host "Version $Version by Berry"
	}



	Function DetermineSwitch
	{

	#	RunDll32.exe InetCpl.cpl,ClearMyTracksByProcess [#]
	#	1		Delete History
	#	2		Delete Cookies
	#	8		Delete Temporary Internet Files
	#	16		Delete Form Data
	#	32		Delete Stored Passwords
	#	255		Delete All
	#	4351	Delete All with the "Also delete files and settings stored by add-ons" options selected
	#
	#	Usage	http://technet.microsoft.com/en-us/library/ff730941.aspx
	#

		switch ($Argument) 
			{ 
			All			{$Global:Action = 255}	#	RunDll32.exe InetCpl.cpl,ClearMyTracksByProcess 255		Delete All 
			AllInfo		{$Global:Action = 4351}	#	RunDll32.exe InetCpl.cpl,ClearMyTracksByProcess 4351	Delete All with the `"Also delete files and settings stored by add-ons`" options selected
			Cookies		{$Global:Action = 2}	#	RunDll32.exe InetCpl.cpl,ClearMyTracksByProcess 2		Delete Cookies 
			Files		{$Global:Action = 8}	#	RunDll32.exe InetCpl.cpl,ClearMyTracksByProcess 8		Delete Temporary Internet Files 
			Form		{$Global:Action = 16}	#	RunDll32.exe InetCpl.cpl,ClearMyTracksByProcess 16		Delete Form Data 
			Help		{$Global:Action = 0}	#	Show this help info 
			History		{$Global:Action = 1}	#	RunDll32.exe InetCpl.cpl,ClearMyTracksByProcess 1		Delete History 
			Passwords	{$Global:Action = 32}	#	RunDll32.exe InetCpl.cpl,ClearMyTracksByProcess 32		Delete Stored Passwords 
			}
	}



	Function DrawWindow
	{	
		$F_Width	= 400
		$F_Height	= 150
		$B_PosX		= 200
		$B_PosY 	= 80
		$B_Width	= 75
		$B_Height	= 23
		$PF32bit	= ${ENV:ProgramFiles}
		$PF64bit	= ${ENV:ProgramFiles(x86)}
		
		
		[void] [System.Reflection.Assembly]::LoadWithPartialName("System.Drawing") 
		[void] [System.Reflection.Assembly]::LoadWithPartialName("System.Windows.Forms") 

		$objForm = New-Object System.Windows.Forms.Form 
		$objForm.Text = "Internet Explorer Cache Cleaner"
		$objForm.Size = New-Object System.Drawing.Size($F_Width,$F_Height) 
		$objForm.StartPosition = "CenterScreen"

		#Determine hardware architecture and select relevant Iexplore.exe
		# 32-bit = [IntPtr]::Size -eq 4
		# 64-bit = [IntPrt]::Size -eq 8
		
		if ([IntPtr]::Size -eq 4)
		{
		$Icon = [system.drawing.icon]::ExtractAssociatedIcon("$PF32bit\Internet Explorer\iexplore.exe")
		}
		Else
		{
		$Icon = [system.drawing.icon]::ExtractAssociatedIcon("$PF64bit\Internet Explorer\iexplore.exe")
		}
			
		$objForm.Icon = $Icon

		$objForm.KeyPreview = $True
		$objForm.Add_KeyDown({if ($_.KeyCode -eq "Enter") {DoExec}})
		$objForm.Add_KeyDown({if ($_.KeyCode -eq "Escape") {DoExit}})

		$OKButton = New-Object System.Windows.Forms.Button
		$OKButton.Location = New-Object System.Drawing.Size($B_PosX,$B_PosY)
		$OKButton.Size = New-Object System.Drawing.Size($B_Width,$B_Height)
		$OKButton.Text = "OK"
		$OKButton.Add_Click({DoExec})
			
		$objForm.Controls.Add($OKButton)

		$CancelButton = New-Object System.Windows.Forms.Button
		$CancelButton.Location = New-Object System.Drawing.Size(($B_PosX+$B_Width),$B_PosY)
		$CancelButton.Size = New-Object System.Drawing.Size($B_Width,$B_Height)
		$CancelButton.Text = "Cancel"
		$CancelButton.Add_Click({DoExit})
		$objForm.Controls.Add($CancelButton)

		$objLabel = New-Object System.Windows.Forms.Label
		$objLabel.Location = New-Object System.Drawing.Size(20,20) 
		$objLabel.Size = New-Object System.Drawing.Size($F_Width,20) 
		$objLabel.Text = "Are you sure you want to clean up the Internet Explorer?"
		
		$objForm.Controls.Add($objLabel) 

		# $objTextBox = New-Object System.Windows.Forms.TextBox 
		# $objTextBox.Location = New-Object System.Drawing.Size(10,40) 
		# $objTextBox.Size = New-Object System.Drawing.Size(260,20) 
		# $objForm.Controls.Add($objTextBox) 

		$objForm.Topmost = $True

		$objForm.Add_Shown({$objForm.Activate()})
		[void] $objForm.ShowDialog()

		$DrawTextBox
	}



	Function DoExec
	{
			#Executing the Internet Explorer Cache whilst using the giving parameter
			RunDll32.exe InetCpl.cpl,ClearMyTracksByProcess $Action
			write-host "Cleaning IE cache - option $Argument"
			$DrawTextBox=$objTextBox.Text
			$objForm.Close()
	}



	Function DoExit
	{
			#Aborting activities
			write-host "Cleaning IE cache aborted"
			$objForm.Close()
	}

	Function GetParam
	{
		if (!$Argument.Count -eq 1)
			{
			ShowHelp
			}
		Else
			{
			If ($Argument -like "Help")
				{
				ShowHelp
				break
				}
			Else
				{
				DetermineSwitch
				DrawWindow
				}
			}
	}
		$Global:Argument = $Args
		GetParam