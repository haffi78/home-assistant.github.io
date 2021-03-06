---
layout: page
title: "LaCrosse Sensor"
description: "Instructions how to integrate LaCrosse sensor data received from Jeelink into Home Assistant."
date: 2017-10-29 15:00
sidebar: true
comments: false
sharing: true
footer: true
logo: home-assistant.png
ha_category: Sensor
ha_release: 0.58
ha_iot_class: "Local Polling"
---

The `lacrosse` sensor platform is using the data provided by a [Jeelink](https://www.digitalsmarties.net/products/jeelink) USB dongle or this [Arduino sketch](https://svn.fhem.de/trac/browser/trunk/fhem/contrib/arduino/36_LaCrosse-LaCrosseITPlusReader.zip).

#### {% linkable_title Tested Devices %}

- Technoline TX 29 IT (temperature only)
- Technoline TX 29 DTH-IT (including humidity)

## {% linkable_title Setup %}

Since the sensor change their ID after each powercycle/battery change you can check what sensor IDs are availble by using the command-line tool `pylacrosse` from the pylacrosse package.

```bash
$ sudo pylacrosse -D /dev/ttyUSB0 scan
```
To use your `lacrosse` compatible sensor in your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
sensor:
  - platform: lacrosse
    sensors:
      sensor_identifier:
        type: SENSOR_TYPE
        id: SENSOR_ID
```

{% configuration %}
  device:
    description: The serial baudrate.
    required: true
    type: string
    default: /dev/ttyUSB0
  baud:
    description: The serial baudrate.
    required: true
    type: int
    default: 57600
  sensors:
    description: A list of your sensors.
    required: true
    type: map
    keys:
      name:
        description: The name of the sensor.
        required: false
        type: string
      type:
        description: "The type of the sensor. Options: `battery`, `humidity`, `temperature`"
        required: true
        type: string
      id:
        description: The LaCrosse Id of the sensor.
        required: true
        type: int
{% endconfiguration %}


## {% linkable_title Examples %}

To setup a lacrosse sensor with multiple sensors, add the following to your `configuration.yaml` file:

{% raw %}
```yaml
# Example configuration.yaml entry
sensor:
  - platform: lacrosse
    device: /dev/ttyUSB0
    baud: 57600
    sensors:
      kitchen_humidity:
        name: Kitchen Humidity
        type: humidity
        id: 72
      kitchen_temperature:
        name: Kitchen Temperature
        type: temperature
        id: 72
      kitchen_lacrosse_battery:
        name: Kitchen Sensor Battery
        type: battery
        id: 72
```
{% endraw %}

