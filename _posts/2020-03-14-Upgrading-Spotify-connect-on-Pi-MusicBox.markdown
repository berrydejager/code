---
layout: post
title: 
date: 2020-03-14 13:37:00 +0100
description:  
img: header/upgrading-spotify-connect-on-pi-musicbox.png
tags: [Raspberry Pi, Raspbian, Bash, PiMusicBox]
---
You might have noticed that when playing music using Spotify-connect on the Pi MusicBox that there is something weird going on. When changing songs or playlists the MusicBox doesn’t follow your commands.

It seems that this is due to an old version of the Librespot files. So login as root, using SSH, on your Pi MusicBox and execute the following command sequence;

```
curl -sSL https://dtcooper.github.io/raspotify/key.asc | sudo apt-key add -v -
echo 'deb https://dtcooper.github.io/raspotify jessie main' | sudo tee /etc/apt/sources.list.d/raspotify.list
apt-get update
apt-get -y install raspotify
ln -f -s /usr/bin/librespot /opt/librespot/librespot
reboot
```
Alternatively you can try;

```
service librespot stop
cd /opt/librespot
wget https://github.com/pimusicbox/librespot/releases/download/v20180529-1e69138/librespot-linux-armhf-raspberry_pi.zip
unzip librespot-linux-armhf-raspberry_pi.zip
rm librespot-linux-armhf-raspberry_pi.zip
service librespot start
```

Now enjoy switching playlists and songs as this will work during Spotify-connect playback.
