---
layout: post
title: Getting TightVNCserver started at boot
date: 2015-12-25 13:37:00 +0100
description: Getting TightVNCserver started at boot
img: header/getting-tightvncserver-started-at-boot.png
tags: [Raspberry Pi, RaspberryPi, RPi, VNC]
---
Getting TightVNCserver started on boot using a RaspberryPi can get some troublesome. So i aggregated the information from various forum-posts to make it work correctly.

Refresh the installation of TightVNCserver

    sudo apt-get purge --auto-remove tightvncserver

Create a TightVNCserver boot script

    sudo nano /etc/init.d/vncboot

Content of the ```/etc/init.d/vncboot``` file;

    #!/bin/sh
    # /etc/init.d/vncboot
    ### BEGIN INIT INFO
    # Provides: vncserver
    # Required-Start: networking
    # Required-Stop:
    # Default-Start: 2 3 4 5
    # Default-Stop: 0 1 6
    # Short-Description: Starts VNC
    # Description:
    ### END INIT INFO

    export USER='pi'

    eval cd ~$USER

    # Check state
    case "$1" in
    start)
    su $USER -c '/usr/bin/vncserver :1 -geometry 1280x720 -depth 24'
    echo "Starting vncserver for $USER"
    ;;
    stop)
    pkill Xtightvnc
    echo "vncserver stopped"
    ;;
    *)
    echo "Usage: /etc/init.d/vncboot {start|stop}"
    exit 1
    ;;
    esac

    exit 0

Refresh the ```/etc/rc#.d``` directories with the correct ```K01vncboot``` files

    sudo update-rc.d -f vncboot remove
    sudo update-rc.d vncboot defaults
