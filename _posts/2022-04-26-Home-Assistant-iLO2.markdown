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
* Add-ons
  *   [Studio Code Server](https://community.home-assistant.io/t/home-assistant-community-add-on-visual-studio-code/107863), web enabled editor with code linting.

# How to configure iLO integration

Adding the [HP_iLO-integration](https://www.home-assistant.io/integrations/hp_ilo/) to your Home Assistent is easy-peasy. 

Although there is, up to now, no GUI way of adding this YAML-based integration the configuration can be done by editing the `/config/configuration.yaml` file.

I use the Studio Code Server add-on for the ease of altering the Home Assistant configuration files.

## Create specific accounts in  he iLO2 interface

For the ease of configuration and not handing out the full access credentials to Home Assistant I opted to create separate accounts on the iLO interface for the handling of the HP_iLO requests.

## Secrets configuration

You can use the `/config/secrets.yaml` file to stash your iLO2 servers' specifics and credentials.

```
hpILOIP01: server01-ilo2.lab.corp
hpILOUsername: readonly
hpILOPassword: jvBqefEcwfm5PeqGkATjh6YJ
```

## Integration configuration

The integration is based on a [YAML](https://yaml.org/) file. The colon-centered syntax of this so-called "human-friendly data serialization language" is very strict. Luckily the Visual Studio Code has a YAML code linting feature to help you out by highlighting syntax errors.  


Add this to your existing `/config/configuration.yaml` file to enable the hp_ilo platform integration.

Please note: the `scan_interval` has been set to 300 seconds to give the iLO2 time to respond for all monitored variables. The default value is set to 30 seconds according to the [scan_interval documentation](https://www.home-assistant.io/docs/configuration/platform_options/#scan-interval).
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

      - name: HYPER03_uid_status
        sensor_type: server_uid_status

      - name: server01_power_readings
        sensor_type: server_power_readings
        unit_of_measurement: "Watts"
        value_template: "{{ ilo_data.present_power_reading[0]}}"

      - name: server01_health_fan_1_speed
        sensor_type: server_health
        unit_of_measurement: "%"
        value_template: '{{ ilo_data.fans["Fan 1"].speed[0] }}'
      - name: server01_health_fan_2_speed
        sensor_type: server_health
        unit_of_measurement: "%"
        value_template: '{{ ilo_data.fans["Fan 2"].speed[0] }}'
      - name: server01_health_fan_3_speed
        sensor_type: server_health
        unit_of_measurement: "%"
        value_template: '{{ ilo_data.fans["Fan 3"].speed[0] }}'
      - name: server01_health_fan_4_speed
        sensor_type: server_health
        unit_of_measurement: "%"
        value_template: '{{ ilo_data.fans["Fan 4"].speed[0] }}'

      - name: server01_temp_ambient
        sensor_type: server_health
        unit_of_measurement: "°C"
        value_template: '{{ ilo_data.temperature["Temp 1"].currentreading[0] }}'
      - name: server01_temp_cpu1
        sensor_type: server_health
        unit_of_measurement: "°C"
        value_template: '{{ ilo_data.temperature["Temp 2"].currentreading[0] }}'
      - name: server01_temp_cpu2
        sensor_type: server_health
        unit_of_measurement: "°C"
        value_template: '{{ ilo_data.temperature["Temp 3"].currentreading[0] }}'
```

## Lovelace card configuration

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

# iLO2 integration reference table

Here you can find all the iLO2 items, which I was able to gather while fiddling around.

Health-at-a-glance - https://server01-ilo2.lab.corp/dhealth.htm
```
{'fans': {'status': 'Ok', 'redundancy': 'Fully Redundant'}, 'temperature': {'status': 'Ok'}, 'vrm': {'status': 'Ok'}, 'power_supplies': {'status': 'Ok', 'redundancy': 'Not Redundant'}, 'drive': {'status': 'Ok'}}
```

Fans - https://server01-ilo2.lab.corp/dhealthf.htm

```
{'label': 'Fan 1', 'zone': 'System', 'status': 'Ok', 'speed': (27, 'Percentage')} 	
```

Temperatures - https://server01-ilo2.lab.corp/dhealtht.htm

Power - https://server01-ilo2.lab.corp/dhealthv.htm

Processors - https://host00-ilo.lab.home/dsysproc.htm

Memory - https://host00-ilo.lab.home/dsysmem.htm

NIC - https://host00-ilo.lab.home/dhealthp.htm

Drives - https://host00-ilo.lab.home/dhealthd.htm

## Outputs

### Heatlth-t-a-glance
{'fans': {'status': 'Ok', 'redundancy': 'Fully Redundant'}, 'temperature': {'status': 'Ok'}, 'vrm': {'status': 'Ok'}, 'power_supplies': {'status': 'Ok', 'redundancy': 'Not Redundant'}, 'drive': {'status': 'Ok'}}

### Fan
{'label': 'Fan 1', 'zone': 'System', 'status': 'Ok', 'speed': (27, 'Percentage')} 


# Home work / Reference information

[HP iLO2 Scripting and Command Line Guide](./assets/pdf/HP ilo 2 Scripting and Command Line Guide.pdf)