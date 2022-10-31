---
layout: post
title: Installing Pihole 5.0 beta on Ubuntu Server
date: 2020-01-26 13:37:00 +0100
description: Do you want to install Pihole 5.x beta on Ubuntu Server?
img: header/installing-pihole-5.0-beta-on-ubuntu-server.gif
tags: [PowerShell, PoSh, Scripting, Windows]
---
Do you want to install Pihole 5.x beta on Ubuntu Server?

#### Preparation

First, you need to gather the installation files in .ISO format for [Ubuntu Server](https://ubuntu.com/download/server) linux distribution. You can either transfer this file to a USB stick, using [Rufus](https://rufus.ie/), or use the .ISO in your hypervisor environment.

#### Updating the Ubuntu Server instance

After installing the Ubuntu Server to your (virtual) machine you need to proceed by getting the Ubuntu Server updated.

You can accomplish this using a set of commands.

```sudo apt-get update```

```sudo apt-get upgrade```

```sudo reboot```

Automating the ```apt-get update```

```sudo nano /nano /etc/apt/apt.conf.d/50unattended-upgrades```

#### Upgrading to Pi-hole beta release

Check the version of the installed Ubuntu Server


```lsb_release -a```

```
No LSB modules are available.
 Distributor ID: Ubuntu
 Description: Ubuntu 18.04.3 LTS
 Release: 18.04
 Codename: bionic
```

#### Installing and configuring the PiHole 4.x software

```curl -sSL https://install.pi-hole.net | bash```

Set/remove password for the web interface

``` pihole -a -p```

#### Upgrading to the beta 5.0

Setting up for the beta release

```echo "release/v5.0" | sudo tee /etc/pihole/ftlbranch```

Upgrade the CORE components to 5.0 beta

```pihole checkout core release/v5.0```

Upgrade the web interface to 5.0 beta

```pihole checkout web release/v5.0```

Add custom entries to the ‘custom dns’

Editing the ```/etc/pihole/custom.list```

restarting DNS resolver 

```pihole restartdns```

#### Optionally, synchronizing custom.list with a secondary PiHole instance

```sudo scp user@pihole01:/etc/pihole/custom.list /etc/pihole/custom.list```

