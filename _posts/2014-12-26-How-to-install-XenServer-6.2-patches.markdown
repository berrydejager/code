---
layout: post
title: How to install XenServer 6.2 patches
date: 2014-12-26 13:37:00 +0100
description:  
img: header/how-to-install-xenserver-6.2-patches.gif
tags: [Bash, XenServer, Citrix]
---
Updating your free XenServer 6.2 install can be a struggle, unless you do your home-work first!

So the choir is downloading the .zip files from Citrix (see links in the XenCenter console) and unpack the files. You only need the ```*.xsupdate``` files, the source files are not needed for updating your environment.

So after uploading the updates, using any SCP/SFTP client to the i.e. ```/root``` directory on the out-dated XenServer, i start the following commands using the shell interface connected to the server to be updated.

This batch of commands firstly 'updates' the update repository, applies the patch (using the hard to remember UUID) and removes the update file again to preserve the precious disk space on the XenServer.

Make sure that you ejected the XenServer Tools CD from every VM you may have defined otherwise the first update fails already.

    xe patch-upload file-name=XS62ESP1.xsupdate
    xe patch-pool-apply uuid=0850b186-4d47-11e3-a720-001b2151a503
    rm -f XS62ESP1.xsupdate

    xe patch-upload file-name=XS62ESP1002.xsupdate
    xe patch-pool-apply uuid=297f2f77-5603-4aaf-9e56-db49512d4592
    rm -f XS62ESP1002.xsupdate

    xe patch-upload file-name=XS62ESP1003.xsupdate
    xe patch-pool-apply uuid=c208dc56-36c2-4e91-b8d7-0246575b1828
    rm -f XS62ESP1003.xsupdate

    xe patch-upload file-name=XS62ESP1005.xsupdate
    xe patch-pool-apply uuid=1c952800-c030-481c-a0c1-d1b45aa19fcc
    rm -f XS62ESP1005.xsupdate

    xe patch-upload file-name=XS62E014.xsupdate
    xe patch-pool-apply uuid=78251ea4-e4e7-4d72-85bd-b22bc137e20b
    rm -f XS62E014.xsupdate

    xe patch-upload file-name=XS62E015.xsupdate
    xe patch-pool-apply uuid=c8b9d332-30e4-4e5e-9a2a-8aaae6dee91a
    rm -f XS62E015.xsupdate

    xe patch-upload file-name=XS62ESP1008.xsupdate
    xe patch-pool-apply uuid=59a75271-12f9-4e6a-8ba2-325c2f5b0b47
    rm -f XS62ESP1008.xsupdate

    xe patch-upload file-name=XS62ESP1009.xsupdate
    xe patch-pool-apply uuid=a24d94e1-326b-4eaa-8611-548a1b5f8bd5
    rm -f XS62ESP1009.xsupdate

    xe patch-upload file-name=XS62ESP1011.xsupdate
    xe patch-pool-apply uuid=7d670435-547c-419a-ab7e-296705a752b8
    rm -f XS62ESP1011.xsupdate

    xe patch-upload file-name=XS62ESP1012.xsupdate
    xe patch-pool-apply uuid=a26964cf-a409-46a4-b94c-66bf6083690f
    rm -f XS62ESP1012.xsupdate

    xe patch-upload file-name=XS62ESP1013.xsupdate
    xe patch-pool-apply uuid=b22d6335-823d-43a6-ba26-28793717125b
    rm -f XS62ESP1013.xsupdate

    xe patch-upload file-name=XS62ESP1014.xsupdate
    xe patch-pool-apply uuid=4fc82e62-b938-407d-a2c6-68c8922f3ec2
    rm -f XS62ESP1014.xsupdate

    xe patch-upload file-name=XS62ESP1015.xsupdate
    xe patch-pool-apply uuid=f4f66d0a-d408-446e-a014-8e793baccb07
    rm -f XS62ESP1015.xsupdate

    xe patch-upload file-name=XS62ESP1016.xsupdate
    xe patch-pool-apply uuid=55444a02-4d97-4d6d-b076-ddbe8697244b
    rm -f XS62ESP1016.xsupdate

Now Bob's your uncle!