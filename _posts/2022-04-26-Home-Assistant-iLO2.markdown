---
layout: post
title: Integrating HP iLO2 entities into Home Assistant.
date: 2022-04-26 13:37:00 +0100
description: Integrating HP iLO2 into Home Assistent dashboards.
img: header/HASS-iLO.png
tags: [HomeAssistant, HASS, HASSio, HP, ML350-G6, iLO2, hp_ilo]
---
Being fond of statistics, I decided to monitor my, old-but-beefy, iLO2-equipped [HP ML350-G6](https://support.hpe.com/hpesc/public/docDisplay?docId=emr_na-c01713311) servers.

The use case for my servers is running a lab environment for educational purposes. Giving easy insight into the statistics helps to keep the usage down to the necessary level only.

I know that the wingspan of the HP_iLO platform, [open-sourced on Github](https://github.com/home-assistant/core/tree/dev/homeassistant/components/hp_ilo), is not only restricted to iLO2. It can work with many more [HP iLO](https://en.wikipedia.org/wiki/HP_Integrated_Lights-Out) versions. However, for this post, the focus is the HP iLO2 only.

The result of this configure reflects the following, however, as always with Home Assistant, your milage may vary. At least you learn from it!

![](/assets/img/HASS-iLO_overview.png)

# What to use?

For maximum functionality, I went for the Home Assistant OS (or Supervised), see; [Home Assistant installation methods](https://www.home-assistant.io/installation/#compare-installation-methods).

Benefits:
* [Supervisor](https://www.home-assistant.io/integrations/hassio/)
* [Studio Code Server](https://community.home-assistant.io/t/home-assistant-community-add-on-visual-studio-code/107863) add-on, web enabled editor with code linting.

# How to configure iLO integration

Adding the [HP_iLO-integration](https://www.home-assistant.io/integrations/hp_ilo/) to your Home Assistent is easy-peasy. 

Although there is, up to now, no GUI way of adding this YAML-based integration the configuration can be done by editing the `/config/configuration.yaml` file.

I use the Studio Code Server add-on for the ease of altering the Home Assistant configuration files.

Please note that when using the latest iLO firmware you have to pay extra performance point, see my blog-post; [HP iLO2 extremely slow over HTTPS](https://code.berrydejager.com/HP-iLO2-extremely-slow-over-HTTPS/)

## Create specific accounts in he iLO2 interface

For the ease of configuration and not handing out the full access credentials to Home Assistant I opted to create separate accounts on the iLO interface for the handling of the HP_iLO requests.

The user management (https://server01-ilo2.lab.corp/dusrpref.htm) on the iLO2 enables you, while logged in using the administrator role, to add/alter accounts.

Checking the sensors

The hp_ilo integration details can be defined in the `configuration.yaml` under `monitored_variables` section.

## System Status - Summary


The Summary on the System Status pages shows:

https://server01-ilo2.lab.corp/dqstat.htm


| Description | Platform integration | Example of returned data | 
| --- | --- | --- |
| Server power | - name: server01_power_status<br />&nbsp;&nbsp;sensor_type: server_power_status | OFF |
| UID Light | - name: server01_power_status<br />&nbsp;&nbsp;sensor_type: server_uid_status | OFF |

---

## System Information - Summary

https://server01-ilo2.lab.corp/dhealth.htm

| Description | Status | Redundancy level |
| --- | --- | --- |
| Fans: | &nbsp; | &nbsp; |
| Temperatures: | &nbsp; | &nbsp; |
| VRMs: | &nbsp; | &nbsp; |
| Power Supplies: | &nbsp; | &nbsp; |
| Drives: | &nbsp; | &nbsp; |

---

## System Information - Fans

https://server01-ilo2.lab.corp/dhealthf.htm

Requesting data: 
``` '{{ ilo_data.fans["Fan 1"].speed[0] }}' ```

Response data: 
``` {'label': 'Fan 1', 'zone': 'System', 'status': 'Ok', 'speed': (26, 'Percentage')}```

From this I distilled the following sensor configuration;

| Description | Location | Platform integration - Status | Platform integration - Speed |
| --- | --- | --- | --- |
| Fan 1: | System Zone |sensor_type: server_health<br />&nbsp;&nbsp;unit_of_measurement: "%"<br />&nbsp;&nbsp;value_template: '\{\{ ilo_data.fans["Fan 1"].status[0] \}\}' | sensor_type: server_health<br />&nbsp;&nbsp;unit_of_measurement: "%"<br />&nbsp;&nbsp;value_template: '\{\{ ilo_data.fans["Fan 1"].speed[0] \}\}' |
| Fan 2: | System Zone |sensor_type: server_health<br />&nbsp;&nbsp;unit_of_measurement: "%"<br />&nbsp;&nbsp;value_template: '\{\{ ilo_data.fans["Fan 2"].status[0] \}\}' | sensor_type: server_health<br />&nbsp;&nbsp;unit_of_measurement: "%"<br />&nbsp;&nbsp;value_template: '\{\{ ilo_data.fans["Fan 2"].speed[0] \}\}' |
| Fan 3: | System Zone |sensor_type: server_health<br />&nbsp;&nbsp;unit_of_measurement: "%"<br />&nbsp;&nbsp;value_template: '\{\{ ilo_data.fans["Fan 3"].status[0] \}\}' | sensor_type: server_health<br />&nbsp;&nbsp;unit_of_measurement: "%"<br />&nbsp;&nbsp;value_template: '\{\{ ilo_data.fans["Fan 3"].speed[0] \}\}' |
| Fan 4: | System Zone |sensor_type: server_health<br />&nbsp;&nbsp;unit_of_measurement: "%"<br />&nbsp;&nbsp;value_template: '\{\{ ilo_data.fans["Fan 4"].status[0] \}\}' | sensor_type: server_health<br />&nbsp;&nbsp;unit_of_measurement: "%"<br />&nbsp;&nbsp;value_template: '\{\{ ilo_data.fans["Fan 4"].speed[0] \}\}' |

---

## System Information - Temperatures

https://server01-ilo2.lab.corp/dhealtht.htm


sensor.hp_ilo_hyper03_exp1
 HP ILO HYPER03_exp1 
 {'label': 'Temp 1', 'location': 'Ambient', 'status': 'Ok', 'currentreading': (28, 'Celsius'), 'caution': (42, 'Celsius'), 'critical': (46, 'Celsius')} 	
 friendly_name: HP ILO HYPER03_exp1

sensor.hp_ilo_hyper03_exp2
HP ILO HYPER03_exp2
	{'label': 'Temp 2', 'location': 'CPU 1', 'status': 'Ok', 'currentreading': (40, 'Celsius'), 'caution': (82, 'Celsius'), 'critical': (83, 'Celsius')} 	
 friendly_name: HP ILO HYPER03_exp2

sensor.hp_ilo_hyper03_exp3
 HP ILO HYPER03_exp3
	{'label': 'Temp 3', 'location': 'CPU 2', 'status': 'Ok', 'currentreading': (40, 'Celsius'), 'caution': (82, 'Celsius'), 'critical': (83, 'Celsius')} 	
 friendly_name: HP ILO HYPER03_exp3

sensor.hp_ilo_hyper03_exp4
 HP ILO HYPER03_exp4
	{'label': 'Temp 4', 'location': 'Memory', 'status': 'Ok', 'currentreading': (42, 'Celsius'), 'caution': (87, 'Celsius'), 'critical': (92, 'Celsius')} 	
 friendly_name: HP ILO HYPER03_exp4

sensor.hp_ilo_hyper03_exp5
 HP ILO HYPER03_exp5
	{'label': 'Temp 5', 'location': 'Memory', 'status': 'Ok', 'currentreading': (36, 'Celsius'), 'caution': (87, 'Celsius'), 'critical': (92, 'Celsius')} 	friendly_name: HP ILO HYPER03_exp5

sensor.hp_ilo_hyper03_exp6
 HP ILO HYPER03_exp6
	{'label': 'Temp 6', 'location': 'Memory', 'status': 'Ok', 'currentreading': (36, 'Celsius'), 'caution': (87, 'Celsius'), 'critical': (92, 'Celsius')} 	friendly_name: HP ILO HYPER03_exp6

sensor.hp_ilo_hyper03_exp7
 HP ILO HYPER03_exp7
	{'label': 'Temp 7', 'location': 'Memory', 'status': 'Ok', 'currentreading': (36, 'Celsius'), 'caution': (87, 'Celsius'), 'critical': (92, 'Celsius')} 	friendly_name: HP ILO HYPER03_exp7

sensor.hp_ilo_hyper03_exp8
 HP ILO HYPER03_exp8
	{'label': 'Temp 8', 'location': 'Memory', 'status': 'Ok', 'currentreading': (39, 'Celsius'), 'caution': (87, 'Celsius'), 'critical': (92, 'Celsius')} 	friendly_name: HP ILO HYPER03_exp8

sensor.hp_ilo_hyper03_exp9
 HP ILO HYPER03_exp9
	{'label': 'Temp 9', 'location': 'Memory', 'status': 'Ok', 'currentreading': (40, 'Celsius'), 'caution': (87, 'Celsius'), 'critical': (92, 'Celsius')} 	friendly_name: HP ILO HYPER03_exp9

sensor.hp_ilo_hyper03_exp10
 HP ILO HYPER03_exp10
 {'label': 'Temp 10', 'location': 'Memory', 'status': 'Ok', 'currentreading': (43, 'Celsius'), 'caution': (87, 'Celsius'), 'critical': (92, 'Celsius')} 	
 friendly_name: HP ILO HYPER03_exp10
 
sensor.hp_ilo_hyper03_exp11
 HP ILO HYPER03_exp11
	{'label': 'Temp 11', 'location': 'Memory', 'status': 'Ok', 'currentreading': (47, 'Celsius'), 'caution': (87, 'Celsius'), 'critical': (92, 'Celsius')} 	
 friendly_name: HP ILO HYPER03_exp11

sensor.hp_ilo_hyper03_exp12
HP ILO HYPER03_exp12
	{'label': 'Temp 12', 'location': 'I/O Board 7', 'status': 'Ok', 'currentreading': (47, 'Celsius'), 'caution': (68, 'Celsius'), 'critical': (73, 'Celsius')} 	
 friendly_name: HP ILO HYPER03_exp12

sensor.hp_ilo_hyper03_exp13
HP ILO HYPER03_exp13
	{'label': 'Temp 13', 'location': 'I/O Board 6', 'status': 'Ok', 'currentreading': (46, 'Celsius'), 'caution': (68, 'Celsius'), 'critical': (73, 'Celsius')} 	
 friendly_name: HP ILO HYPER03_exp13

sensor.hp_ilo_hyper03_exp14
HP ILO HYPER03_exp14
	{'label': 'Temp 14', 'location': 'I/O Board 5', 'status': 'Ok', 'currentreading': (43, 'Celsius'), 'caution': (68, 'Celsius'), 'critical': (73, 'Celsius')} 	
 friendly_name: HP ILO HYPER03_exp14

sensor.hp_ilo_hyper03_exp15
HP ILO HYPER03_exp15
	{'label': 'Temp 15', 'location': 'I/O Board 4', 'status': 'Ok', 'currentreading': (41, 'Celsius'), 'caution': (68, 'Celsius'), 'critical': (73, 'Celsius')} 	
 friendly_name: HP ILO HYPER03_exp15

sensor.hp_ilo_hyper03_exp16
HP ILO HYPER03_exp16
	{'label': 'Temp 16', 'location': 'I/O Board 3', 'status': 'Ok', 'currentreading': (39, 'Celsius'), 'caution': (68, 'Celsius'), 'critical': (73, 'Celsius')} 	
 friendly_name: HP ILO HYPER03_exp16

sensor.hp_ilo_hyper03_exp17
HP ILO HYPER03_exp17
	{'label': 'Temp 17', 'location': 'I/O Board 2', 'status': 'Ok', 'currentreading': (37, 'Celsius'), 'caution': (68, 'Celsius'), 'critical': (73, 'Celsius')} 	
 friendly_name: HP ILO HYPER03_exp17

sensor.hp_ilo_hyper03_exp18
HP ILO HYPER03_exp18
	{'label': 'Temp 18', 'location': 'I/O Board 1', 'status': 'Ok', 'currentreading': (37, 'Celsius'), 'caution': (68, 'Celsius'), 'critical': (73, 'Celsius')} 	
 friendly_name: HP ILO HYPER03_exp18

sensor.hp_ilo_hyper03_exp19
HP ILO HYPER03_exp19
	{'label': 'Temp 19', 'location': 'CPU', 'status': 'Ok', 'currentreading': (35, 'Celsius'), 'caution': (87, 'Celsius'), 'critical': (92, 'Celsius')} 	
 friendly_name: HP ILO HYPER03_exp19

sensor.hp_ilo_hyper03_exp20
 HP ILO HYPER03_exp20
	{'label': 'Temp 20', 'location': 'Memory', 'status': 'Ok', 'currentreading': (38, 'Celsius'), 'caution': (87, 'Celsius'), 'critical': (92, 'Celsius')} 	
 friendly_name: HP ILO HYPER03_exp20
 
sensor.hp_ilo_hyper03_exp21
 HP ILO HYPER03_exp21
	{'label': 'Temp 21', 'location': 'Storage', 'status': 'Ok', 'currentreading': (35, 'Celsius'), 'caution': (60, 'Celsius'), 'critical': (65, 'Celsius')} 	
 friendly_name: HP ILO HYPER03_exp21

sensor.hp_ilo_hyper03_exp22
 HP ILO HYPER03_exp22
	{'label': 'Temp 22', 'location': 'System', 'status': 'Ok', 'currentreading': (69, 'Celsius'), 'caution': (110, 'Celsius'), 'critical': (115, 'Celsius')} 	
 friendly_name: HP ILO HYPER03_exp22

sensor.hp_ilo_hyper03_exp23
 HP ILO HYPER03_exp23
	{'label': 'Temp 23', 'location': 'System', 'status': 'Ok', 'currentreading': (46, 'Celsius'), 'caution': (87, 'Celsius'), 'critical': (92, 'Celsius')} 	
 friendly_name: HP ILO HYPER03_exp23

sensor.hp_ilo_hyper03_exp24
 HP ILO HYPER03_exp24
	{'label': 'Temp 24', 'location': 'System', 'status': 'Ok', 'currentreading': (49, 'Celsius'), 'caution': (87, 'Celsius'), 'critical': (92, 'Celsius')} 	
 friendly_name: HP ILO HYPER03_exp24

| Description | Location | Platform integration: Status | Platform integration: Reading | Caution | Critical |
| --- | --- | --- | --- | --- | --- |
| Temp 1: | Ambient | sensor_type: server_health<br />&nbsp;&nbsp;value_template: '\{\{ ilo_data.temperature["Temp 1"].status \}\}' | sensor_type: server_health<br />&nbsp;&nbsp;unit_of_measurement: "°C"<br />&nbsp;&nbsp;value_template: '\{\{ ilo_data.temperature["Temp 1"].currentreading[0] \}\}' | 42C | 46C |

---

## System Information - Power

https://server01-ilo2.lab.corp/dhealthv.htm

---

## System Information - Processors

https://server01-ilo2.lab.corp/dsysproc.htm

---

## System Information - Memory

https://server01-ilo2.lab.corp/dsysmem.htm

---

## System Information - NIC

https://server01-ilo2.lab.corp/dhealthp.htm

---

## System Information - Drives

https://server01-ilo2.lab.corp/dhealthd.htm

---

# Secrets configuration

You can use the `/config/secrets.yaml` file to stash your iLO2 servers' specifics and credentials.

```
hpILOIP01: server01-ilo2.lab.corp
hpILOUsername: readonly
hpILOPassword: jvBqefEcwfm5PeqGkATjh6YJ
```

# Integration configuration

The integration is based on a [YAML](https://yaml.org/) file. The colon-centered syntax of this so-called "human-friendly data serialization language" is very strict. Luckily the Visual Studio Code has a YAML code linting feature to help you out by highlighting syntax errors. 

From the temperature overview in ILO2, in my case `https://server01-ilo2.lab.corp/dhealtht.htm`, over you can pick your temperature readings. 

While observing the `CPU 1` and `CPU 2` i noticed that they stick at 40 degrees celsius at all times, measured over different servers and scenarios. This is the reason I monitor the `Temp 19: CPU Zone` and `Tmep 21: Storage Zone` to give meaningfull info to the dashboard.

Please note: the `scan_interval` has been set to 300 seconds to give the iLO2 time to respond for all monitored variables. The default value is set to 30 seconds according to the [scan_interval documentation](https://www.home-assistant.io/docs/configuration/platform_options/#scan-interval).


Add the `sensor` section to your existing `/config/configuration.yaml` file to enable the hp_ilo platform integration.

```
sensor:
 - platform: hp_ilo
 host: !secret hpILOIP01
 username: !secret hpILOUsername
 password: !secret hpILOPassword
 scan_interval: 300
 monitored_variables:

 - name: server01_power_status
 sensor_type: server_power_status

 - name: server01_uid_status
 sensor_type: server_uid_status

 - name: server01_power_readings
 sensor_type: server_power_readings
 unit_of_measurement: "Watts"
 value_template: "\{\{ ilo_data.present_power_reading[0]\}\}"

 - name: server01_health_fan_1_speed
 sensor_type: server_health
 unit_of_measurement: "%"
 value_template: '\{\{ ilo_data.fans["Fan 1"].speed[0] \}\}'
 
 - name: server01_health_fan_2_speed
 sensor_type: server_health
 unit_of_measurement: "%"
 value_template: '\{\{ ilo_data.fans["Fan 2"].speed[0] \}\}'
 
 - name: server01_health_fan_3_speed
 sensor_type: server_health
 unit_of_measurement: "%"
 value_template: '\{\{ ilo_data.fans["Fan 3"].speed[0] \}\}'
 
 - name: server01_health_fan_4_speed
 sensor_type: server_health
 unit_of_measurement: "%"
 value_template: '\{\{ ilo_data.fans["Fan 4"].speed[0] \}\}'

 - name: server01_temp_ambient
 sensor_type: server_health
 unit_of_measurement: "°C"
 value_template: '\{\{ ilo_data.temperature["Temp 1"].currentreading[0] \}\}'
 
 - name: server01_temp_cpu1
 sensor_type: server_health
 unit_of_measurement: "°C"
 value_template: '\{\{ ilo_data.temperature["Temp 2"].currentreading[0] \}\}'
 
 - name: server01_temp_cpu2
 sensor_type: server_health
 unit_of_measurement: "°C"
 value_template: '\{\{ ilo_data.temperature["Temp 3"].currentreading[0] \}\}'
```

# Lovelace card configuration

Showing your harvested data into a card on the overview is the next step. 

Firstly navigate in the browser to the [overview dashboard](http://homeassistant.local:8123/lovelace/default_view).

Create a card and navigate to the 'show code editor`. From here you can overwrite the content with the code mentioned below.

This conditional card will only be shown when the iLO reports itself when the power status is `ON`.

```
type: conditional
conditions:
 - entity: sensor.hp_ilo_server01_power_status
 state: 'ON'
card:
 title: iLO2 server01
 state_color: true
 header:
 type: graph
 entity: sensor.hp_ilo_server01_temp_ambient
 detail: 2
 type: entities
 entities:
 - entity: sensor.hp_ilo_server01_power_status
 name: 'Power status changed:'
 icon: mdi:power
 secondary_info: last-changed
 - entity: sensor.hp_ilo_server01_uid_status
 name: 'UID LED changed:'
 icon: mdi:led-on
 secondary_info: last-changed
 - entity: sensor.hp_ilo_server01_temp_ambient
 name: 'Ambient Temp updated:'
 secondary_info: last-updated
 - entity: sensor.hp_ilo_server01_temp_cpu1
 name: 'CPU 1 Temp updated:'
 secondary_info: last-updated
 - entity: sensor.hp_ilo_server01_temp_cpu2
 name: 'CPU 2 Temp updated:'
 secondary_info: last-updated
 - entity: sensor.hp_ilo_server01_power_readings
 secondary_info: last-updated
 name: 'Power consumption updated:'
 icon: mdi:power-plug
 - entity: sensor.hp_ilo_server01_health_fan_1_speed
 secondary_info: last-updated
 name: 'Fan 1 updated:'
 icon: mdi:fan
 - entity: sensor.hp_ilo_server01_health_fan_2_speed
 secondary_info: last-updated
 name: 'Fan 2 updated:'
 icon: mdi:fan
 - entity: sensor.hp_ilo_server01_health_fan_3_speed
 secondary_info: last-updated
 name: 'Fan 3 updated:'
 icon: mdi:fan
 - entity: sensor.hp_ilo_server01_health_fan_4_speed
 secondary_info: last-updated
 name: 'Fan 4 updated:'
 icon: mdi:fan
```

# Home work / Reference information

[HP iLO2 Scripting and Command Line Guide](./assets/pdf/HP ilo 2 Scripting and Command Line Guide.pdf)