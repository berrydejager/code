---
layout: post
title: Installing VICE on a Raspberry Pi
date: 2016-10-30 13:37:00 +0100
description: Getting your Raspberry Pi upgraded to a C64 experience
img: header/installing-vice-on-raspberry-pi.gif
tags: [Raspberry-Pi, VICE, Raspbian, Commodore 64, C64]
---
Upgrading your RPi to a Commodore 64 experience is the best upgrade you can get.

#### Getting down to business

Prior to installing VICE on your Jessie distribution of Raspbian you need to alter the sources list.


    sudo nano /etc/apt/sources.list

    # Add the following lines on the end of the file

    deb http://ftp.nl.debian.org/debian/ jessie main contrib

Execute the following commands to completely install VICE including the ROM files.

#### Run the source update

    apt-get update

#### Install VICE from the newly added source

    apt-get install vice

#### Fetch, unpack and copy ROM files

    cd ~
    wget http://www.zimmers.net/anonftp/pub/cbm/crossplatform/emulators/VICE/old/vice-1.5-roms.tar.gz
    tar -xvzf vice-1.5-roms.tar.gz
    sudo cp -a vice-1.5-roms/data/* /usr/lib/vice/

#### Start VICE

    x64

Enjoy the sleepless nights...
