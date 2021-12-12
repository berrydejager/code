---
layout: post
title: Office 365 setting mailbox permissions
date: 2016-09-30 13:37:00 +0100
description:  
img: header/office-365-setting-mailbox-permissions.png
tags: [Microsoft, Office 365, PowerShell]
---
When editing mailbox permissions and archiving email to a .PST files can be cumbersome. I have come up with a script to make live a little more easy.

    $AdminUser = admin
    $UserCredential = Get-Credential
    $Session = New-PSSession -ConfigurationName Microsoft.Exchange -ConnectionUri https://outlook.office365.com/powershell-liveid/ -Credential $UserCredential -Authentication Basic -AllowRedirection
    Import-PSSession $Session

    $MBXS = Get-Recipient -RecipientType UserMailbox

    ForEach ($MBX in $MBXS)
    {
        Write-Host $MBX.name
        Add-MailboxPermission $MBX.name -User $AdminUser -AccessRights FullAccess -InheritanceType All
        Write-Host $MBX.name
        $PSTFile = "C:\_PST\Mailbox_$MBX.name.pst" ## add Your new PST file name path
        Write-Host $PSTFile
        $outlook = New-Object -ComObject outlook.application
        $namespace = $Outlook.GetNameSpace("MAPI")
        $NameSpace.AddStore($PSTFile) ## Add the new PST to the Current profile
    }