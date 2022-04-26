---
layout: post
title: Home Assistant iLO2 entities.
date: 2022-04-26 13:37:00 +0100
description: Intergrating HP iLO2 into Home Assistent dashboards.
img: header/HASS-iLO.png
tags: [HomeAssistant, HASS, HASSio, HP, iLO2]
---
# Why

Monitor the iLO2 servers that I use for my lab environment.

# What

Home Assistant as OS installation

Benefits:
*   Add-ons
    *   Studio Code Server, web enabled editor with code linting.

# How 

## Create specific accounts in iLO2 interface


## Secrets configuration

You can use the `/config/secrets.yaml` file to stash your iLO2 servers specifics and credentials.

```
# Use this file to store secrets like usernames and passwords.
# Learn more at https://www.home-assistant.io/docs/configuration/secrets/
hpILOIP01: server01-ilo2.lab.corp
hpILOUsername: readonly
hpILOPassword: jvBqefEcwfm5PeqGkATjh6YJ
```

## Integration configuration

```
# Loads default set of integrations. Do not remove.
default_config:

# Text to speech
tts:
  - platform: google_translate

automation: !include automations.yaml
script: !include scripts.yaml
scene: !include scenes.yaml
sensor:
  - platform: hp_ilo
    host: !secret hpILOIP01
    username: !secret hpILOUsername
    password: !secret hpILOPassword
    scan_interval: 300
    monitored_variables:
      - name: server01_power_status
        sensor_type: server_power_status

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
  title: HYPER01
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