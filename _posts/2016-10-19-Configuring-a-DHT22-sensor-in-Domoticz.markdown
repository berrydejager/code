---
layout: post
title: Configuring a DHT22 sensor in Domoticz
date: 2016-10-19 13:37:00 +0100
description: How to configure a DHT22 sensor in Domoticz to measure temperature and humidity values.
img: header/configuring-a-dht22-sensor-in-domoticz.png
tags: [PowerShell, PoSh, Scripting, Windows]
---
How to configure a DHT22 sensor in Domoticz to measure temperature and humidity values.

#### Installation of Domoticz

First get Domoticz installed on the Raspberry Pi.

```sudo curl -L install.domoticz.com | sudo bash```

Choose between HTTP and HTTPS protocol definition for connecting to the Domoticz web console.

Select the installation folder, default set to ```/home/pi/domoticz/```

The settings, HTTP and HTTPS web interface definitions, are kept in the daemon script. You can easily edit this script by using;

```sudo nano /etc/init.d/domoticz.sh```

#### Introducing the DHT22 sensor script to the Domoticz installation.

```
#!/bin/bash
# Domoticz server
# Please replace [SERVER] and [PORT] to reflect the Domoticz server specifications
SERVER="[SERVER]:[PORT]"
# DHT IDX
# The number of the IDX in the list of peripherals
DHTIDX="18"
# DHTPIN
# The GPIO or connects DHT11
DHTPIN="22"
# TMPFILE: path for temporary file in place to avoid the RAMDRIVE
TMPFILE="/var/tmp/dht22.txt"
# Retry loop, try 5 times
cpt=1
while [ $cpt -lt 6 ]
do
TEMP=""
echo Attempt: $cpt
sudo nice -20 /home/pi/Adafruit_Python_DHT/examples/AdafruitDHT.py 22 $DHTPIN > $TMPFILE
TEMP=$(cat $TMPFILE | awk -F "=" '{print $2}' | awk -F "." '{print $1}')
HUM=$(cat $TMPFILE | awk -F "=" '{print $3}' | awk -F "." '{print $1}')
echo "Temp=" $TEMP
echo "Humid=" $HUM
# Send data
curl -s -i -H 'Accept: application/json' "http://$SERVER/json.htm?type=command&param=udevice&idx=$DHTIDX&nvalue=0&svalue=$TEMP;$HUM;0"
TEMP=""
HUM=""
exit 0
cpt=$(($cpt+1))
done
exit 1
```

