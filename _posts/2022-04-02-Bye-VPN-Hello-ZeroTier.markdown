---
layout: post
title: Bye VPN! Hello ZeroTier!
date: 2022-04-02 13:37:00 +0100
description: While looking for a solution to access my servers behind a NAT/routed network i stumbled upon ZeroTier. This leverages the VPN capabiities to a out-of-this-world next dimension, everything is super simple and cloud driven. 
img: header/zerotier.png
tags: [ZeroTier, VPN, GL.iNet, Brume, GL-MV1000]
---
# Why do i need to have a remoting solution?

During this C19-pandemic working from home (WFH) wasn't really getting the work/life balance in an equilibrium, so i decided to rent a office for myself. This all seems nice and dandy up to the point that no internet connectivity is available at the remote office. Juggling around with a USB tethered phone to my laptop wasn't cutting it in the long run as i needed to have multiple machines hooked onto ["The System"](https://en.wikipedia.org/wiki/Internet).

Fiddling around with a Raspberry Pi and using this as an USB-tethering bridge made me investigating a more stable solution; the tiny but mighty [GL.iNet Brume GL-MV1000](https://www.gl-inet.com/products/gl-mv1000/). Just plugin the USB of the smartphone, connect LAN port to a swith, hooked some machines onto the switch and away we go.

Still having a little caveat; connecting from home to the office was not feasible as the telecom provider does NAT on the 4G connection, resulting into the inabilitiy to open port for a VPN connection. I was stumped for a while... until a fellow Commodore enthusiast pointed [ZeroTier](https://www.zerotier.com/) to me.

## What is ZeroTier?

"ZeroTier is a smart Ethernet switch for planet Earth. It’s a distributed network hypervisor built atop a cryptographically secure global peer to peer network. It provides advanced network virtualization and management capabilities on par with an enterprise SDN switch, but across both local and wide area networks and connecting almost any kind of app or device."

So far for the marketing fluff, in essence it allows you to create a virtual network in the cloud and install the [ZeroTier software](https://www.zerotier.com/download/) onto a Linux/Windows/MacOS (virtual) machine, as a docker, a router and even onto NAS devices.

![](/assets/img/ZT_NetworkGraphic_Homepage.png)

## Why choosing ZeroTier?

This virtual network is acccessible as long as you connected to "The System". The best part of all this is that you don't need to worry about router configuration of opening ports, forwarding traffic etc. , hence it really fits my need.

It checked all my requirements

[x] Open source [on GitHub.com](https://github.com/zerotier)
[x] hassle-free, no port forwarding
[x] [Free plan](https://www.zerotier.com/pricing/), no purchase/creditcard registration required
[x] A vast variety of platform support
[x] [API-enabled and well documented](https://docs.zerotier.com/central/v1/)
[x] Support for HashiCorp [Terraform](https://docs.zerotier.com/terraform/quickstart)

## How does it work?

Essentially it requires just a few steps;'

1. Create a [ZeroTier account](https://accounts.zerotier.com/auth/realms/zerotier/protocol/openid-connect/registrations?client_id=zt-central&redirect_uri=https%3A%2F%2Fmy.zerotier.com%2Fapi%2F_auth%2Foidc%2Fcallback&response_type=code&scope=openid+profile+email+offline_access&state=state) to access your admin console and get a 16-digit network ID. Create as many networks as you like and each will be assigned a 16-digit network ID.
2. [Download ZeroTier](https://www.zerotier.com/download/) on any device to get a unique 10-digit node address and enter your 16-digit network ID into the join network field on the device to request access to your network.
3. Check the Auth checkbox on your admin console when your 10-digit node address presents itself.

## What did i use?

* VMWare ESXi 6.7 as a hypervisor platform
* ZeroTier account, which is free
* a Linux Mint 20.3 virtual machine at the local location
* a Linux Mint 20.3 virtual machine at the remote location
* Creating a ZeroTier network between these two VMs

## Installing Linux Mint in a VM

Cinnamon -> RDP -> Linux Mint interface
XFCE -> no Linux Mint interface

Just install from the ISO and make sure that the installed distro is up to date

    sudo apt update && sudo apt full-upgrade -y

## Installing remote desktop on Linux Mint

    sudo apt install xrdp

## Creating a ZeroTier network

First create a virtual network to span accross two or multiple sites.

| NETWORK ID        | NETWORK NAME      | DESCRIPTION   | SUBNET            | NODES | CREATED       |
| ---               | ---               | ---           | ---               | ---   | ---           | 
| 3efa5cb78a7f3d2d  | berserk_licklider |               | 192.168.196.0/24  | 0/0   | 2022-04-02    |

## Installing the ZeroTier software

Downloading and fixing the installer script to support Linux Mint / Ubuntu (Focal) distribution.

    curl -s https://install.zerotier.com | sed 's/xenial/focal/g' > zerotier_install.sh 

Giving the downloaded script executing permissions

    chmod +x zerotier_install.sh

Starting the installer script

    ./zerotier_install.sh 

Joining the network 

    sudo zerotier-cli join 3efa5cb78a7f3d2d

## Start networking

Enjoy the simplicity of ZetoTier.
