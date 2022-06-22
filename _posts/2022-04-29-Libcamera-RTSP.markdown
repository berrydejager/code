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

When plugging in a monitor to the (mini) HDMI port of the Raspberry Pi you can test the camera by executing a test program.

The monitor will shortly show the video which comes from the connected camera.

This shows if you have correctly installed the camera to the Pi.

```
libcamera-hello
```

## Preparing software

General updating

```
sudo apt update && sudo apt -y full-upgrade
```

Installing VLC ,this can take a while.

```
sudo apt install -y vlc
```

Allow VLC to be run as root. This is needed otherwise the streaming can not commence.
```
sudo sed -i 's/geteuid/getppid/' /usr/bin/vlc
```


Setting up `stream.sh` file

For setting the camera configuration you can set a number of parameters, see [documentation](https://www.raspberrypi.com/documentation/accessories/camera.html)

You can use nano to create the `stream.sh` file

```
sudo nano ~/stream.sh
```

Contents of the `stream.sh` file

When needed you can add `--vflip` and/or `--hflip` for correcting the view.

```
#!/bin/bash
libcamera-vid -t 0 --inline --width 1920 --height 1080 --framerate 15 -o - | cvlc -vvv stream:///dev/stdin --sout '#rtp{sdp=rtsp://:8554/stream}' :demux=h264
```

Enable the script file to be executed:

```
sudo chmod +x /home/pi/stream.sh
```

Enabling the script as service

```
sudo nano /lib/systemd/system/stream.service
```

And write in it:
```
[Unit]
Description=Custom Webcam Streaming Service
After=multi-user.target

[Service]
Type=simple
ExecStart=/home/pi/stream.sh
Restart=on-abort

[Install]
WantedBy=multi-user.target
```

Set correct permissions on the service file: 

```
sudo chmod 644 /lib/systemd/system/stream.service


Creating the service on the system
```
sudo systemctl enable stream.service
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


