---
title: "Binary Sensor Group"
description: "Instructions for how to setup binary_sensor groups within Home Assistant."
ha_category:
  - Binary Sensor
ha_release: 2021.10
ha_iot_class: Local Push
ha_quality_scale: internal
ha_domain: group
---

The group binary_sensor platform lets you combine multiple binary_sensors into one entity. This integration presents itself as a single binary sensor in the UI. This allows the UI to use the text and icon of the type of sensor you are grouping. The sensor doesn't know what device class it should be, so this should be set to match the device class of the items in the group.

## Configuration

To enable this platform in your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
binary_sensor:
    - platform: group
      name: Patio Doors
      device_class: opening
      entities:
        - binary_sensor.door_left_contact
        - binary_sensor.door_right_contact
```

{% configuration %}
entities:
  description: A list of entities to be included in the group.
  required: true
  type: [string, list]
name:
  description: The name of the group. Defaults to "Binary Sensor Group".
  required: false
  type: string
unique_id:
  description: An ID that uniquely identifies this group. If two binary_sensors have the same unique ID, Home Assistant will raise an error.
  required: false
  type: string
all:
  description: Set this to `true` if the group state should only turn *on* if **all** grouped entities are *on*.
  required: false
  type: boolean
  default: false
device_class:
  description: Sets the [device class](/integrations/binary_sensor/) of this binary sensor group, changing the device state and icon that is displayed in the frontend.
  required: false
  type: string
{% endconfiguration %}

## Group behavior

Group behavior differs depending on if the `all` option is `false` (the default) or `true`.
If `all` is `false`(the default):
- Group state is `unavailable` if all group members are `unavailable`
- Otherwise, group state is `unknown` if all group members are `unknown`
- Otherwise, group state is `on` if at least one group member is `on`
- Otherwise, group state is `off`

If `all` is `true`:
- Group state is `unavailable` if all group members are `unavailable`
- Otherwise, group state is `unknown` if at least one group member is `unknown` or `unavailable`
- Otherwise, group state is `off` if at least one group member is `off`
- Otherwise, group state is `on`
