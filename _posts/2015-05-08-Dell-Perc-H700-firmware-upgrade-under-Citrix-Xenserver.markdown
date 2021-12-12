---
layout: post
title: Dell Perc H700 firmare upgrade under Citrix XenServer
date: 2015-05-08 13:37:00 +0100
description: Updating the Dell Perc H700 raid controller firmware can be a hideous job. Let me explain how i got this done while having Citrix XenServer installed.
img: header/dell-perc-h700-firmware-upgrade-under-citrix-xenserver.png
tags: [Dell, Citrix, XenServer, Perc, H700, firmware]
---
Updating the Dell Perc H700 raid controller firmware can be a hideous job. Let me explain how i got this done while having Citrix XenServer installed.

### Preparation

First download the [SAS-RAID_Firmware_C3X7D_LN_12.10.6-0001_A12.BIN](http://downloads.dell.com/FOLDER01560661M/1/SAS-RAID_Firmware_C3X7D_LN_12.10.6-0001_A12.BIN) file from the Dell Support website.

### Installation

Make sure that the file is available in your ~ directory. Either use wget of (win)scp to get the .bin file.

    [root@R210 ~]# ls
    SAS-RAID_Firmware_C3X7D_LN_12.10.6-0001_A12.BIN  support.tar.bz2

Change the access permissions of the file in order to make it executable.

    [root@R210 ~]# chmod 777 SAS-RAID_Firmware_C3X7D_LN_12.10.6-0001_A12.BIN

Extract the executable .BIN file.

    [root@R210 ~]# ./SAS-RAID_Firmware_C3X7D_LN_12.10.6-0001_A12.BIN --extract RAIDFW
    Successfully extracted to RAIDFW

Check the content of the folder.

    [root@R210 ~]# cd RAIDFW/

    [root@R210 RAIDFW]# ls
    00-secupd-dell.rules  doRPM.sh                en.prop      libstorelibir-2.so  mc.txt       PIEConfig.sh  smbiosHelp.txt  spsetup.sh         srvadmin-storelib-sysfs-7.2.0-4.1.1.el4.x86_64.rpm  Version.txt
    98-secupdusb.rules    dupdisneyinstall.sh     getSystemId  libstorelibir.so    package.xml  PIEInfo.txt   spconfig.xml    sputility.bin      svmExeMsg.xsl
    buildVer.sh           duppmdatacollector.bin  hapi         libstorelib.so      payload      sasdupie      sphelp.txt      spUtilityHelp.txt  uni-eol.txt

Execute the firmware update using the ```SASDUPIE``` tool.

    [root@R210 RAIDFW]# ./sasdupie -u -s payload/

    <?xml version="1.0" encoding="UTF-8"?><SVMExecution lang="en"><Device vendorID="1000" deviceID="0079" subDeviceID="1f17" subVendorID="1028" bus="1" device="0" function="0" display="PERC H700 Integrated Controller 0"><Application componentType="FRMW" version="12.10.0-0025" display="PERC H700 Integrated Controller 0 Firmware"><Package version="12.10.6-0001"/><SPStatus result="true"><Message id="0">The operation was successful. </Message></SPStatus></Application></Device><RebootRequired>1</RebootRequired></SVMExecution>

As stated ```The operation was successful.``` and the system will reboot.