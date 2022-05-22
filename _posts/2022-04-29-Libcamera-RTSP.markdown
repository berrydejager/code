---
layout: post
title: How to stream over RTSP using RPi Bullseye
date: 2022-04-29 13:37:00 +0100
description: How to stream over RTSP using Raspberry Pi 'Bullseye' combined with libcamera as a daemon.
img: header/libcamera-rtsp.png
tags: [Raspberry PI, RPi, Bullseye, libcamera]
---

# Why

As `raspicam` is deprecated in Raspbian OS 11 Bullseye, the obvious alternative is `libcamera`.

# What to use?

# What not to use?

The Raspberry PI Zero W / W2

It doesn't detect all camera types correctly, see forum post ["Pi Zero W2 and Bullseye: *** no cameras available ***"](https://forums.raspberrypi.com/viewtopic.php?t=323462&sid=13ce0a463a98a00285ea12348abd5803&start=25)

# How to configure

## Preparing hardware

Connecting the camera to the Raspberry PI can be done by using the CSI (Camera Serial Interface) connector. 

![1](/assets/img/libcamera-rtsp_rpi_cam_01.png)![2](/assets/img/libcamera-rtsp_rpi_cam_02.png)

## Testing hardware

```
libcamera-hello
```

## Preparing software

General updating

```
sudo apt update && sudo apt -y full-upgrade
```

Installing VLC ,this can take a while...

```
sudo apt install
```

Setting up `stream.sh` file

For setting the camera configuration you can set a number of parameters, see [documentation](https://www.raspberrypi.com/documentation/accessories/camera.html)

```
#!/bin/bash
libcamera-vid -t 0 -- width 1920 --height 1080 --hflip --inline --framerate 15 -o - | cvlc -vvv stream:///dev/stdin --sout '#r
tp{sdp=rtsp://:8554/stream}' :demux=h264
```

Enabling the script as service

```
sudo systemctl enable stream.service
```

Setting the correct permission on the service
```
sudo chmod 644 /lib/systemd/system/stream.service
```

Allow VLC to be run as root. This is needed otherwise the streaming can not commence.
```
sudo sed -i 's/geteuid/getppid/' /usr/bin/vlc
```

Starting the service

```
sudo service stream start
```

Checking the status of the service
```
sudo service stream status
```

## Checking the streaming on your desktop

Start VLC media player and navigate to `Media` -> `Open network Stream`  or press CTRL+N.

Enter the URL as follows:
rtsp://raspberrypi.local:8554/stream


