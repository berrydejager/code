---
layout: post
title: Installing XenServer 6.5 on Dell PowerEdge R210
date: 2015-05-08 13:37:00 +0100
description: Installing XenServer 6.5 on Dell PowerEdge R210 
img: header/installing-xenServer-6.5-on-dell-poweredge-r210.gif
tags: [Citrix, XenServer, Installation, Dell, PowerEdge, R210]
---
Firstly we set the XenServer root partition to 16GB (opposed to the default 4GB, which is kinda tight), see [Major IO blog](https://major.io/2012/01/13/xenserver-6-disable-gpt-and-get-a-larger-root-partition/)

Press ```F2``` on install to enter the shell

    shell
    vi /opt/xensource/installer/constants.py

Change the following lines

line #89 ```GPT_SUPPORT = False```

Line #92 ```root_size = 16384```


### Set NTP servers

NTP Servers from [NTP Pool Project](http://www.pool.ntp.org/en/)

0.nl.pool.ntp.org


### Update the XenServer 
Detailed description in [How to install XenServer 6.5 patches](https://code.berrydejager.com/how-to-install-xenserver-6-5-patches/)

Install the Dell OMSA software on the XenServer partition.

### Step #1: Temporarily disable the Citrix yum repository.

As of this post, Citrix's repo does not seem to be working properly.

To do so, let's temporarily move the Citrix.repo file out of the ```/etc/yum.repos.d``` folder.
Alternatively, you can also disable the repo within the file itself.

```mv /etc/yum.repos.d/Citrix.repo /root/Citrix.repo```

### Step #2: Clean up using YUM

```yum clean all```

### Step #3: Install the latest supported Dell OMSA Repository:

```wget -q -O - http://linux.dell.com/repo/hardware/Linux_Repository_14.12.00/bootstrap.cgi | bash```

### Step #4: Install Dell OpenManage Server Administrator

```yum --enablerepo=base install srvadmin-all -y```

### Step # 5: Open port 1311 in iptables

```nano /etc/sysconfig/iptables```

Now add the following line above the last line which says icmp-host-prohibited:

```
-A RH-Firewall-1-INPUT -m conntrack --ctstate NEW -m udp -p udp --dport 161 -j ACCEPT
-A RH-Firewall-1-INPUT -m conntrack --ctstate NEW -m tcp -p tcp --dport 1311 -j ACCEPT
```

### Step #6: Restart iptables

```service iptables restart```

### Step 7: Start Dell OpenManage Server Administrator

```/opt/dell/srvadmin/sbin/srvadmin-services.sh start```

### Step #8: Move the Citrix.repo file back to the original location:

``` mv /root/Citrix.repo /etc/yum.repos.d```

### Step #9: You can now access OMSA via your favourite browser:

```https://Your.IP.Address.Here:1311```