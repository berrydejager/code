---
layout: post
title: Restarting explorer.exe from the context menu
date: 2018-06-05 13:37:00 +0100
description: In order to restart the explorer.exe in an user friendly fashion, the idea was born to arrange this from the desktop context menu.
img: header/restarting-explorer.exe-from-context-menu.gif
tags: [Microsoft, Windows, Explorer, Desktop, Context menu]
---
In order to restart the explorer.exe in an user friendly fashion, the idea was born to arrange this from the desktop context menu.

This is achievable by triggering a restart of the explorer.exe process. So first the explorer.exe process is killed and restarted again, effectively building up the user interface again while a user remains logged on.

I created a registry item which allows you to right-click on the desktop and select ```Refresh-Workspace```.

#### Content of the registry file

```
Windows Registry Editor Version 5.00
[HKEY_CLASSES_ROOT\DesktopBackground\Shell\Refresh Workspace]
@="Refresh Workspace"
"Position"="Bottom"
"Icon"="%systemroot%\\system32\\SHELL32.dll,238"
"MUIVerb"="Refresh Workspace"
[HKEY_CLASSES_ROOT\DesktopBackground\Shell\Refresh Workspace\command]
@="cmd.exe /c taskkill /f /im explorer.exe & start explorer.exe"
```