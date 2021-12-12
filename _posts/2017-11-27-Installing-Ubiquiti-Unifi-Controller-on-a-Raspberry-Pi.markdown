---
layout: post
title: Installing Ubiquiti Unifi Controller on a Raspberry Pi
date: 2017-11-27 13:37:00 +0100
description: Installing Ubiquiti Unifi Controller on a Raspberry Pi
img: header/installing-ubiquiti-unifi-controller-on-a-raspberry-pi.png
tags: [PowerShell, PoSh, Scripting, Windows]
---
Just got my new Ubiquity Unifi gear at home and i need to install the controller software. The first option that came to mind is a Raspberry Pi, sporting Raspbian ‘Stretch’.

#### Updating Raspbian

To get the ```apt-key``` command successfully downloading the Ubiquity GPG-key on Raspbian Stretch, you need to install the ```dirmngr``` manually. Don’t forget to update the repository first!

```sudo apt-get update```

```sudo apt-get install dirmngr``` 

**Please note**: APT-KEY is deprecated and this isn’t a final solution but merely a workaround.

#### Preparing Raspbian sources list

Adding the Unifi repository to the sources list

```echo 'deb http://www.ubnt.com/downloads/unifi/debian unifi5 ubiquiti' | sudo tee -a /etc/apt/sources.list.d/ubnt.list > /dev/null```

```sudo apt-key adv --keyserver keyserver.ubuntu.com --recv C0A52C50```

```sudo apt-get update```

#### Installing the Unifi controller software

```sudo apt-get install unifi -y```

#### Disable the default MongoDB

```echo 'ENABLE_MONGODB=no' | sudo tee -a /etc/mongodb.conf > /dev/null```

#### Install Oracle Java 8

```sudo apt-get install oracle-java8-jdk -y```

```sudo cp /lib/systemd/system/unifi.service /etc/systemd/system/```

```sudo sed -i '/^\[Service\]$/a Environment=JAVA_HOME=/usr/lib/jvm/jdk-8-oracle-arm32-vfp-hflt'  /etc/systemd/system/unifi.service```

#### Reboot your Pi

```sudo reboot```

After the reboot you can HTTPS to port 8443 on your Raspberry Pi:

```https://[raspberrypi]:8443```
