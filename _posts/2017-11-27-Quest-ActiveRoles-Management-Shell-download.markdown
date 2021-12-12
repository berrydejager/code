---
layout: post
title: Quest ActiveRoles Management Shell download
date: 2017-11-27 13:37:00 +0100
description: Quest ActiveRoles is a collection of very useful PowerShell cmdlets for Active Directory
img: header/quest-activeroles-management-shell-download.png
tags: [PowerShell, PoSh, Scripting, Windows, Quest, ActiveRolesManagement]
---
Quest ActiveRoles is a collection of very useful PowerShell cmdlets for Active Directory. They used to be offered for free by Quest Software (now owned by Dell), but have since after version 1.5.1 (as far as I know) started charging money for the cmdlets in later versions. Around November 2014, Dell removed the download links.

Here are download links for the x64 and x86 versions of the Quest ActiveRoles AD Management Shell version 1.5.1 (last free version).

** Please note**: before installing, you will be able to see that the file is signed by Quest, so the files are legit.

They’re wrapped in zip files, iside there is a MSI file with the same name (signed by Quest).

*	[64-bit version: Quest ActiveRolesManagementShellforActiveDirectoryx64 151.zip](/assets/bin/Quest_ActiveRolesManagementShellforActiveDirectoryx64_151.zip)
*	[32-bit version: Quest ActiveRolesManagementShellforActiveDirectoryx86 151.zip](/assets/bin/Quest_ActiveRolesManagementShellforActiveDirectoryx86_151.zip)

The Quest Activeroles cmdlets as of February 2012 started requiring PowerShell version 2 or higher. The latest publicly available version was 1.5.1 on that Quest.com page. These cmdlets also work against Windows Server 2003 non-R2 domain controllers without Active Directory Web Services.

#### Command to add the Quest ActiveRoles snap-in manually

To add the Quest ActiveRoles AD management snap-in manually, in order to import all the Quest cmdlets, you can use the following command, so you don’t have to start the specific shell/host environment they provide:

```Add-PSSnapin Quest.ActiveRoles.ADManagement```

See Getting computer names from AD using Powershell for an example with the Get-QADComputer cmdlet or Getting usernames from Active Directory with PowerShell for Get-QADUser.

#### Usefull / Handy powershell commands using the Quest ActiveRoles

Getting the limit for retreiving information:

```Get-QADPSSnapinSettings - DefaultSizeLimit```

The default size is set to 1000 results. This can easily be extended by using the following command:

```Set-QADPSSnapinSettings - DefaultSizeLimit 10000```

```Get-QADGroupMember ‘domainname.corp\group01’  -indirect | where-Object -FilterScript {($_.Type -match “user”) -and ($_.DN -notmatch “Disabled”) -and ($_.DN -notmatch “Trash”)} | measure```

