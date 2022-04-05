---
layout: post
title: Bye bye VPN! Hello ZeroTier!
date: 2022-04-02 13:37:00 +0100
description: While looking for a solution to access my servers behind a NAT/routed network, I stumbled upon ZeroTier. It leverages the VPN capabilities to an out-of-this world dimension, as everything is super simple and cloud-driven. 
img: header/zerotier.png
tags: [ZeroTier, VPN, GL.iNet, Brume, GL-MV1000]
---
# Why do I need to have a remoting solution?

During this C19-pandemic working from home (WFH) didn't get the work/life balance in equilibrium, so I decided to rent an office (WFHO) for myself. This all seems nice and dandy, up to the point that no internet connectivity is available at the remote office. Juggling around with a USB tethered phone to my laptop wasn't cutting it in the long run as I needed to have multiple machines hooked onto [The System](https://en.wikipedia.org/wiki/Internet).

Fiddling around with a Raspberry Pi and using this as a USB-tethering bridge made me investigate a more stable solution; the tiny but mighty [GL.iNet Brume GL-MV1000](https://www.gl-inet.com/products/gl-mv1000/). Just plug in the USB of the smartphone, connect LAN port to a switch, hook some machines onto the switch, and away we go.

Still having a little caveat; connecting from home to the office was not feasible as the telecom provider does NAT on the 4G connection, resulting in to the inability to open ports for a VPN connection. I was really stumped for a while. until a fellow [Commodore](https://en.wikipedia.org/wiki/Commodore_International)-enthusiast pointed out [ZeroTier](https://www.zerotier.com/) to me.

# What is ZeroTier?

"ZeroTier is a smart Ethernet switch for planet Earth. It's a distributed network hypervisor built atop a cryptographically secure global peer to peer network. It provides advanced network virtualization and management capabilities on par with an enterprise SDN switch, but across both local and wide area networks and connecting almost any kind of app or device."

So far for the marketing fluff, in essence, it allows you to create a virtual network in the cloud and install the [ZeroTier software](https://www.zerotier.com/download/) onto a Linux/Windows/MacOS (virtual) machine, as a docker, a router and even onto NAS devices.

![](/assets/img/ZT_NetworkGraphic_Homepage.png)

# Why go for ZeroTier?

This virtual network is accessible as long as you connect to "The System". The best part of all this is that you don't need to worry about router configuration of opening ports, forwarding traffic etc., hence it fits my need.

It checked all my requirements

[x] Open source [on GitHub.com](https://github.com/zerotier)

[x] hassle-free, no port forwarding

[x] [Free plan](https://www.zerotier.com/pricing/), no purchase/creditcard registration required

[x] A large variety of platform support

[x] [API-enabled and well documented](https://docs.zerotier.com/central/v1/)

[x] Support for HashiCorp [Terraform](https://docs.zerotier.com/terraform/quickstart)

# How does it work?

Essentially it requires just a few steps;

1. Create a [ZeroTier account](https://accounts.zerotier.com/auth/realms/zerotier/protocol/openid-connect/registrations?client_id=zt-central&redirect_uri=https%3A%2F%2Fmy.zerotier.com%2Fapi%2F_auth%2Foidc%2Fcallback&response_type=code&scope=openid+profile+email+offline_access&state=state) to access your admin console and get a 16-digit network ID. Create as many networks as you like and each will be assigned a 16-digit network ID.
2. [Download ZeroTier](https://www.zerotier.com/download/) on any device to get a unique 10-digit node address and enter your 16-digit network ID into the join network field on the device to request access to your network.
3. Check the Auth checkbox on your admin console when your 10-digit node address presents itself.



# What did I use?

* VMWare ESXi 6.7 as a hypervisor platform
* ZeroTier Basic account, which is free see; [pricing plan](https://www.zerotier.com/pricing/)
* a Linux Mint 20.3 virtual machine at the local location
* a Linux Mint 20.3 virtual machine at the remote location
* Creating a ZeroTier network between these two VMs

The free basic plan comes with;

* Up to 50(!) members on the virtual private network.
* Unlimited amount of network to be configured on your tenant.
* A single ZeroTier admin account
* Support is based on the large community, helping eachother out is what i like.

If you don't like the ZeroTier Hosted Controller you can go for the Open Source plan and [sport your own controller](https://docs.zerotier.com/self-hosting/network-controllers/) and gain the unlimited admin accounts as a bonus.

Using this Basic plan it gave me access to my devices on the other side of the line, good enough.

## Installing Linux Mint in a VM

When selecting a Linux Mint distribution and you want to RDP into is, please take the following in account;

* Cinnamon, renders the nice Linux Mint interface
* Mate or XFCE, renders in a default X interface

Just install from the ISO and make sure that the installed distro is up to date

    sudo apt update && sudo apt full-upgrade -y

## Installing RDP on Linux Mint

    sudo apt install xrdp

## Creating a ZeroTier network

First, create a virtual network to span across two or multiple sites.

| NETWORK ID        | NETWORK NAME      | DESCRIPTION   | SUBNET            | NODES | CREATED       |
| ---               | ---               | ---           | ---               | ---   | ---           | 
| 3efa5cb78a7f3d2d  | berserk_licklider |               | 192.168.196.0/24  | 0/0   | 2022-04-02    |

## Installing the ZeroTier software

The default Zerotier installation method doesn't take a newer version of Ubunt/Linux Mint in account. Just a little addition to the default method takes care of this.

Downloading and fixing the installer script to support Linux Mint / Ubuntu (Focal) distribution.

    curl -s https://install.zerotier.com | sed 's/xenial/focal/g' | bash

Next up, joining the network 

    sudo zerotier-cli join 3efa5cb78a7f3d2d

## Start networking

When logged on to the [ZeroTier web interface](https://my.zerotier.com/) you need to 'Auth' your newly added device to the virtual network.

When adding another device to the ZeroTier network those device are able to communicate with eachother, even though they are behind a NAT or firewalled solution like in my WFHO 4G-scenario.

![](/assets/img/ZT_Members.png)

Enjoy the simplicity of ZeroTier.
