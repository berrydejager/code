---
layout: post
title: Bye VPN! Hello ZeroTier!
date: 2021-04-02 13:37:00 +0100
description: While looking for a solution to access my servers behind a NAT/routed network i stumbled upon ZeroTier. This leverages the VPN capabiities to a out-of-this-world next dimension, everything is super simple and cloud driven. 
img: header/zerotier.png
tags: [ZeroTier, VPN]
---
# Why do i need to have a remoting solution?

## What is ZeroTier?

ZeroTier is a smart Ethernet switch for planet Earth.

It’s a distributed network hypervisor built atop a cryptographically secure global peer to peer network. It provides advanced network virtualization and management capabilities on par with an enterprise SDN switch, but across both local and wide area networks and connecting almost any kind of app or device.

## Why choosing ZeroTier?

It has everything you need;

* It is open source
* A free, no purchase/creditcard registration, plan 
* A vast variety of platform support
* API enabled, see https://docs.zerotier.com/central/v1/
* HashiCorp's Terraform intergration

# How does it work?

# What did i use?

* VMWare ESXi 6.7 as a hypervisor platform
* ZeroTier account, which is free
* a Linux Mint 20.3 virtual machine at the local location
* a Linux Mint 20.3 virtual machine at the remote location
* Creating a ZeroTier network between these two VMs

# How to use the setup?

## Installing Linux Mint in a VM

Cinnamon -> RDP -> Linux Mint interface
XFCE -> no Linux Mint interface

Just install from the ISO and make sure that the installed distro is up to date

    sudo apt update && sudo apt full-upgrade -y

## Installing remote desktop on Linux Mint

    sudo apt install xrdp

## Creating a ZeroTier network
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