---
layout: post
title: Pi-hole, the easy way of adblocking.
date: 2016-09-26 13:37:00 +0100
description: Pi-Hole; the easy way to ad-blocking, without configuring an ad-blocker per device.
img: header/pihole-the-easy-way-to-adblocking.png
tags: [Raspberry Pi, RPi, Pi-hole, ad-blocking]
---
[Pi-Hole](https://pi-hole.net/), the easy way to ad-blocking, without configuring an ad-blocker per device. Even the smartphone, tablet and other devices can benefit from this easily. Just plug-in a Raspberry Pi Zero, wired, into the network. Install and configure the Pi-Hole and change your router to point the primary DNS to the Pi-Hole to black hole those annoying incoming "informative" search results, ads for products that you recently purchased and references to items that your are not planning to buy after all.

### Requirements hardware:
[Raspberry Pi Zero](https://www.raspberrypi.org/)
[USB2LAN adapter (AX8872 chipset)](http://www.dx.com/p/usb-2-0-10-100mbps-rj45-lan-ethernet-network-adapter-dongle-34691#.VpE04-ykrCQ)
[USB to microUSB OTG Converter Shim](https://thepihut.com/products/usb-to-microusb-otg-converter-shim)
2GB (micro) SD card, minimal class 6

### Requirements software:
Raspian Jessie Lite (I used [2016-05-27-raspbian-jessie-lite.img](https://www.raspberrypi.org))

Make sure that the date and time zone is set properly.

    sudo /etc/init.d/ntp stop
    sudo raspi-config
    date -s "24 sep 2016 00:18:00"
    sudo /etc/init.d/ntp start

The NTP daemon will now perform a precision time sync over time.

### Installation:
SSH into the Raspberry Pi and execute the following command

    sudo curl -L https://install.pi-hole.net | bash

This will install the latest version of the Pi-Hole. At the time of writing; Pi-hole version is V2.9.1 / Web-Admin version is v1.4.

Also it will automatically check for the dependencies;

    dnsutils
    bc
    dnsmasq
    lighttpd
    php5-common
    php5-cgi
    php5
    netcat

### White-listing
Add (additional) domains to the Pi-Hole white-list by executing the following commands:

    ###
    ### Default white listing
    ###
    pihole -w raw.githubusercontent.com mirror1.malwaredomains.com sysctl.org zeustracker.abuse.ch s3.amazonaws.com hosts-file.net ransomwaretracker.abuse.ch
    ###
    ### Customized white listing
    ###
    # bol.com
    pihole -w partnerprogramma.bol.com
    # crashlytics.com
    pihole -w settings.crashlytics.com
    # conversive.nl
    pihole -w ant.conversive.nl
    # dartsearch.net
    pihole -w dartsearch.net clickserve.dartsearch.net
    # ds1.nl
    pihole -w ds1.nl
    # everesttech.net
    pihole -w pixel.everesttech.net
    # gjtech.net
    pihole -w adblock.gjtech.net
    # googleadservices.com
    pihole -w googleadservices.com 4.afs.googleadservices.com pagead2.googleadservices.com partner.googleadservices.com www.googleadservices.com
    # imailo.nl
    pihole -w ads.imailo.nl
    # imrworldwide.com
    pihole -w secure-us.imrworldwide.com
    # intercom.io
    pihole -w intercom.io
    # martkplaats.nl
    pihole -w admarkt.marktplaats.nl
    # msftncsi.com
    pihole -w msftncsi.com www.msftncsi.com
    # scorecardresearch.com
    pihole -w sb.scorecardresearch.com
    # snmmd.nl
    pihole -w cts.snmmd.nl sss.snmmd.nl
    # spotify.com
    pihole -w spclient.wg.spotify.com
    # tradedoubler.com
    pihole -w clk.tradedoubler.com pdt.tradedoubler.com
    # tradetracker.net
    pihole -w tc.tradetracker.net
    # zanox.com
    pihole -w ad.zanox.com

### Black-listing

    # Microsoft Windows 10 - Talking to home
    pihole -b dns.msftncsi.com ipv6.msftncsi.com win10.ipv6.microsoft.com ipv6.msftncsi.com.edgesuite.net a978.i6g1.akamai.net win10.ipv6.microsoft.com.nsatc.net en-us.appex-rf.msn.com v10.vortex-win.data.microsoft.com client.wns.windows.com wildcard.appex-rf.msn.com.edgesuite.net v10.vortex-win.data.metron.life.com.nsatc.net wns.notify.windows.com.akadns.net americas2.notify.windows.com.akadns.net travel.tile.appex.bing.com www.bing.com any.edge.bing.com fe3.delivery.mp.microsoft.com fe3.delivery.dsp.mp.microsoft.com.nsatc.net ssw.live.com ssw.live.com.nsatc.net login.live.com login.live.com.nsatc.net directory.services.live.com directory.services.live.com.akadns.net bl3302.storage.live.com skyapi.live.net bl3302geo.storage.dkyprod.akadns.net skyapi.skyprod.akadns.net skydrive.wns.windows.com register.mesh.com BN1WNS2011508.wns.windows.com settings-win.data.microsoft.com settings.data.glbdns2.microsoft.com OneSettings-bn2.metron.live.com.nsatc.net watson.telemetry.microsoft.com watson.telemetry.microsoft.com.nsatc.net

### Update the Pi-Hole
Occasionally an update will be available for you to install

    sudo pihole -up

Resulting in, with possibly other version numbers;

    ::: Checking for updates...
    ::: Pi-hole version is v2.9.3 (Latest version is v2.9.4)
    ::: Web Admin version is v1.4.2 (Latest version is v1.4.3.1)
    :::
    ::: An update is available for Pi-Hole base files and the Web Admin. Both will be updated!
    ::: Fetching latest changes from GitHub...
    ...
    ...

Now you have a working Pi-Hole with a good set of workable white listed domains. Again the optional are up to you to implement.

This way we have installed Raspbian in a minimal set which fits on a 2GB microSD card.

    pi@PiHole:~ $ df -h

    Filesystem      Size  Used Avail Use% Mounted on
    /dev/root       1.8G  1.1G  533M  68% /
    devtmpfs        237M     0  237M   0% /dev
    tmpfs           242M     0  242M   0% /dev/shm
    tmpfs           242M  4.4M  237M   2% /run
    tmpfs           5.0M  4.0K  5.0M   1% /run/lock
    tmpfs           242M     0  242M   0% /sys/fs/cgroup
    /dev/mmcblk0p1   63M   21M   43M  33% /boot

Using the Pi-Hole:

Actually there isn't that much you need to do about it. Maybe tweaking around with whitelisting but you can easily add you own white listing using the ```pihole -w -f [domainname.tld]``` syntaxis.

What could be interesting to see what the pi-hole rejects for you, with this information you able to tune your blacklisting more closely. Open a SSH connection to your Raspberry Pi and start looking into the log files.

    tail -f /var/log/pihole.log | grep -i "/etc/pihole/gravity.list"

This will let you peek into the log file while showing every new entry.

For further tuning/tweaking of the Pi-Hole you can use the following commands.

Here a list of the pihole parameters;

    pihole -w, whitelist Whitelist domains
    pihole -w, whitelist -d, --delmode Remove domains from the whitelist
    pihole -w, whitelist -nr, --noreload Update Whitelist without refreshing dnsmasq
    pihole -w, whitelist -f, --force Force updating of the hosts files, even if there are no changes
    pihole -w, whitelist -q, --quiet output is less verbose
    pihole -w, whitelist -h, --help Show this help dialog
    pihole -w, whitelist -l, --list Display your whitelisted domains
    pihole -b, blacklist Blacklist domains
    pihole -b -d, --delmode Remove domains from the blacklist
    pihole -b -nr, --noreload Update blacklist without refreshing dnsmasq
    pihole -b -f, --force Force updating of the hosts files, even if there are no changes
    pihole -b -q, --quiet output is less verbose
    pihole -b -h, --help Show this help dialog
    pihole -b -l, --list Display your blacklisted domains
    pihole -d, debug Start a debugging session if having trouble
    pihole -f, flush Flush the pihole.log file
    pihole -ud, updateDashboard Update the web dashboard manually
    pihole -up, updatePihole Update Pi-hole
    pihole -g, updateGravity Update the list of ad-serving domains
    pihole -s, setupLCD Automatically configures the Pi to use the 2.8 LCD screen to display stats
    pihole -c, chronometer Calculates stats and displays to an LCD
    pihole -c -j, --json output stats as JSON formatted string
    pihole -c -h, --help isplay this help text
    pihole -h, help Show this help dialog
    pihole -v, version Show current versions
    pihole -q, query Query the adlists for a specific domain
    pihole uninstall Uninstall Pi-Hole from your system
