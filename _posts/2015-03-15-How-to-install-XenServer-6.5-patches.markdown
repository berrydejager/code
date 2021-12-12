---
layout: post
title: How to install XenServer 6.5 patches
date: 2015-03-15 13:37:00 +0100
description: How to install XenServer 6.5 patches
img: header/how-to-install-xenserver-6.5-patches.png
tags: [PowerShell, PoSh, Scripting, Windows]
---
Updating your free XenServer 6.5 install can be a struggle, unless you do your home-work first!

So the choir is downloading the .zip files from Citrix (see links in the XenCenter console) and unpack the files.

You only need the ```*.xsupdate``` files, the source files are not needed for updating your environment.

So after uploading the updates, using any SCP/SFTP client to the i.e. ```/root``` directory on the out-dated XenServer, i start the following commands using the shell interface connected to the server to be updated.

This batch of commands firstly 'updates' the update repository, applies the patch (using the hard to remember UUID) and removes the update file again to preserve the precious disk space on the XenServer.

Make sure that you ejected the XenServer Tools CD from every VM you may have defined otherwise the first update fails already.

    xe patch-upload file-name=XS65E001.xsupdate
    xe patch-pool-apply uuid=9f9d57ff-3a04-4385-9744-f961b44a1db4
    rm -f XS65E001.xsupdate

    xe patch-upload file-name=XS65E002.xsupdate
    xe patch-pool-apply uuid=0fedb090-7d7a-4dce-afac-34d56d4c9aff
    rm -f XS65E002.xsupdate

    xe patch-upload file-name=XS65E003.xsupdate
    xe patch-pool-apply uuid=5200911d-5f79-4149-abca-0556af77b14d
    rm -f XS65E003.xsupdate

    xe patch-upload file-name=XS65E005.xsupdate
    xe patch-pool-apply uuid=492ca007-bf7b-454f-8e5c-63a991a52449
    rm -f XS65E005.xsupdate