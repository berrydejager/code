---
layout: post
title: Home Assistant iLO2 entities.
date: 2022-04-26 13:37:00 +0100
description: Intergrating HP iLO2 into Home Assistent dashboards.
img: header/HASS-iLO.png
tags: [HomeAssistant, HASS, HASSio, HP, ML350G6, iLO2, hp_ilo]
---
Being fond of statistics,I decided to monitor my, old-but-beefy, iLO2-equipped HP ML360-G6 servers. 

My use-case for the servers is running a lab environment for educational purposes. Giving easy insight to the statistics helps keeping the usage down to necessary-level only.

I know that the wingspan of the HP_iLO platform, [open-sourced on Github](https://github.com/home-assistant/core/tree/dev/homeassistant/components/hp_ilo), is not only restricted to iLO2.

# What do i use?

For maximum functionality i went for the Home Assistant OS (or Supervised), see; [Home Assistant installation methods](https://www.home-assistant.io/installation/#compare-installation-methods).

Benefits:
* [Supervisor](https://www.home-assistant.io/integrations/hassio/)
* Add-ons
  *   [Studio Code Server](https://community.home-assistant.io/t/home-assistant-community-add-on-visual-studio-code/107863), web enabled editor with code linting.

# How to configure iLO integration

Adding the [HP_iLO-integration](https://www.home-assistant.io/integrations/hp_ilo/) to your Home Assistent is easy-peasy. 

Although there is no GUI way of addding the intergration the configuration can be done by editing the `/config/configuration.yaml` file.

I use the Studio Code Server add-on for the ease of altering the Home Assistant configuration files.

## Create specific accounts in iLO2 interface

For the ease of configuration and not handing out the full access credentials to Home Assistant I opted to create seperate accounts on the iLO interface for the handling of the HP_iLO requests.

## Secrets configuration

You can use the `/config/secrets.yaml` file to stash your iLO2 servers specifics and credentials.

```
hpILOIP01: server01-ilo2.lab.corp
hpILOUsername: readonly
hpILOPassword: jvBqefEcwfm5PeqGkATjh6YJ
```

## Integration configuration

```
sensor:
  - platform: hp_ilo
    host: !secret hpILOIP01
    username: !secret hpILOUsername
    password: !secret hpILOPassword
    scan_interval: 120
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

Here you find all the iLO2 items which i was able to find while fiddling around.

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


# Reference information

[HP iLO2 Scripting and Command Line Guide](./assets/pdf/HP ilo 2 Scripting and Command Line Guide.pdf)