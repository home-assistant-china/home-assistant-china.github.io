---
layout: page
title: "Xiaomi Binary Sensor"
description: "Instructions how to setup the Xiaomi binary sensors within Home Assistant."
date: 2017-07-21 16:34
sidebar: true
comments: false
sharing: true
footer: true
logo: xiaomi.png
ha_category: Binary Sensor
ha_release: "0.50"
ha_iot_class: "Local Push"
---

`xiaomi` 传感器平台允许你在 HA 里使用[小米](http://www.mi.com/) 传感器

使用此组件时，你必须已经设置[小米多功能网关](/components/xiaomi/).


### {% linkable_title Type of sensors supported %}

| Name                              | ZigBee entity       | Model no.            | States                            | Event          | Event key                               | Event values                                                                                                             |
|-----------------------------------|---------------------|----------------------|----------------------------------------------------|-----------------------------------------|--------------------------------------------------------------------------------------------------------------------------|
| 人体传感器 (1st gen)           | motion              | RTCGQ01LM            | on, off                           | `motion`       |                                         |                                                                                                                          |
| 人体传感器 (2nd gen)           | sensor_motion.aq2   | RTCGQ11LM            | on, off                           | `motion`       |                                         |                                                                                                                          |
| 门窗感应器 (1st gen)           | magnet              | WSDCGQ01LM           | on, off                           |                |                                         |                                                                                                                          |
| 门窗感应器 (2nd gen)           | sensor_magnet.aq2   | MCCGQ11LM            | on, off                           |                |                                         |                                                                                                                          |
| 烟雾传感器                     | smoke               | JTYJ-GD-01LM/BW      | on, off                           |                |                                         |                                                                                                                          |
| 天然气泄漏传感器                | natgas              | JTQJ-BF-01LM/BW      | on, off                           |                |                                         |                                                                                                                          |
| 浸水传感器                     | sensor_wleak.aq1    | SJCGQ11LM            | on, off                           |                |                                         |                                                                                                                          |
| 开关 (1st gen)                | switch              | WXKG01LM             | on (thru long_click_press), off   | `click`        | `click_type`                            | `long_click_press`, `long_click_release`, `hold`, `single`, `double`                                                     |
| 开关 (2nd gen)                | sensor_switch.aq2   | WXKG11LM             | off (always)                      | `click`        | `click_type`                            | `single`, `double`                                                                                                       |
| Aqara 无线开关 (单控)          | 86sw1               | WXKG03LM             | off (always)                      | `click`        | `click_type`                            | `single`                                                                                                                 |
| Aqara 无线开关 (双控)          | 86sw2               | WXKG02LM             | off (always)                      | `click`        | `click_type`                            | `single`, `both`                                                                                                         |
| 小米魔方控制器                  | cube                | MFKZQ01LM            | off (always)                      | `cube_action`  | `action_type`, `action_value` (rotate)  | `flip90`, `flip180`, `move`, `tap_twice`, `shake_air`, `swing`, `alert`, `free_fall`, `rotate` (degrees at action_value) |


### {% linkable_title Automation examples %}
自动化示例

#### {% linkable_title Motion %}

```yaml
- alias: If there is motion and its dark turn on the gateway #开夜灯
light
  trigger:
    platform: state
    entity_id: binary_sensor.motion_sensor_158d000xxxxxc2
    from: 'off'
    to: 'on'
  condition:
    condition: numeric_state
    entity_id: sensor.illumination_34ce00xxxx11
    below: 300
  action:
    - service: light.turn_on
      entity_id: light.gateway_light_34ce00xxxx11
      data:
        brightness: 5
    - service: automation.turn_on
      data:
        entity_id: automation.MOTION_OFF
- alias: If there no motion for 5 minutes turn off the gateway light #无人状态5分钟后关夜灯
  trigger:
    platform: state
    entity_id: binary_sensor.motion_sensor_158d000xxxxxc2
    from: 'on'
    to: 'off'
    for:
      minutes: 5
  action:
    - service: light.turn_off
      entity_id: light.gateway_light_34ce00xxxx11
    - service: automation.turn_off
      data:
        entity_id: automation.Motion_off
```

#### {% linkable_title Door and/or Window %}

```yaml
- alias: If the window is open turn off the radiator #开窗关暖气
  trigger:
    platform: state
    entity_id: binary_sensor.door_window_sensor_158d000xxxxxc2
    from: 'off'
    to: 'on'
  action:
    service: climate.set_operation_mode
    entity_id: climate.livingroom
    data:
      operation_mode: 'Off'
- alias: If the window is closed for 5 minutes turn on the radiator again #关窗5分钟后开暖气
  trigger:
    platform: state
    entity_id: binary_sensor.door_window_sensor_158d000xxxxxc2
    from: 'on'
    to: 'off'
    for:
      minutes: 5
  action:
    service: climate.set_operation_mode
    entity_id: climate.livingroom
    data:
      operation_mode: 'Smart schedule'
```

#### {% linkable_title Smoke %}

```yaml
- alias: Send notification on fire alarm #失火发送消息推送
  trigger:
    platform: state
    entity_id: binary_sensor.smoke_sensor_158d0001574899
    from: 'off'
    to: 'on'
  action:
    - service: notify.html5
      data:
        title: Fire alarm!
        message: Fire/Smoke detected!
    - service: xiaomi_aqara.play_ringtone
      data:
        gw_mac: xxxxxxxxxxxx
        ringtone_id: 2
        ringtone_vol: 100
```

#### {% linkable_title Gas %}

```yaml
- alias: Send notification on gas alarm #煤气泄漏发送消息推送
  trigger:
    platform: state
    entity_id: binary_sensor.natgas_sensor_158dxxxxxxxxxx
    from: 'off'
    to: 'on'
  action:
    - service: notify.html5
      data_template:
        title: Gas alarm!
        message: 'Gas with a density of {% raw %}{{ states.binary_sensor.natgas_sensor_158dxxxxxxxxxx.attributes.density }}{% endraw %} detected.'
```

#### {% linkable_title Xiaomi Wireless Button %}

可触发的事件（有效的控制）有单击 `single`，双击 `double`，持续按住 `hold`，点按后长按 `long_click_press`以及长按后松开 `long_click_press`。绿米(Aqara) 的方型开关只支持单击及双击。注意判定为双击的 2 次按动时间间隔较长。（译者注：因此容易出现误判）

```yaml
- alias: Toggle dining light on single press #单击亮灯
  trigger:
    platform: event
    event_type: click
    event_data:
      entity_id: binary_sensor.switch_158d000xxxxxc2
      click_type: single
  action:
    service: switch.toggle
    entity_id: switch.wall_switch_left_158d000xxxxx01
- alias: Toggle couch light on double click #双击亮灯
  trigger:
    platform: event
    event_type: click
    event_data:
      entity_id: binary_sensor.switch_158d000xxxxxc2
      click_type: double
  action:
    service: switch.toggle
    entity_id: switch.wall_switch_right_158d000xxxxx01
- alias: Let a dog bark on long press #长按网关发出狗叫
  trigger:
    platform: event
    event_type: click
    event_data:
      entity_id: binary_sensor.switch_158d000xxxxxc2
      click_type: long_click_press
  action:
    service: xiaomi_aqara.play_ringtone
    data:
      gw_mac: xxxxxxxxxxxx
      ringtone_id: 8
      ringtone_vol: 8
```

#### {% linkable_title Xiaomi Cube %}

支持的魔方控制器控制有翻转90° `flip90`，翻转180° `flip180`，平移 `move`，双击 `tap_twice`， 悬空摇晃 `shake_air`，摇摆 `swing`，警报 `alert`，下落 `free_fall` 和旋转 `rotate`.

```yaml
- alias: Cube event flip90
  trigger:
    platform: event
    event_type: cube_action
    event_data:
      entity_id: binary_sensor.cube_15xxxxxxxxxxxx
      action_type: flip90
  action:
    - service: light.turn_on
      entity_id: light.gateway_light_28xxxxxxxxxx
      data:
        color_name: "springgreen"
- alias: Cube event flip180
  trigger:
    platform: event
    event_type: cube_action
    event_data:
      entity_id: binary_sensor.cube_15xxxxxxxxxxxx
      action_type: flip180
  action:
    - service: light.turn_on
      entity_id: light.gateway_light_28xxxxxxxxxx
      data:
        color_name: "darkviolet"
- alias: Cube event move
  trigger:
    platform: event
    event_type: cube_action
    event_data:
      entity_id: binary_sensor.cube_15xxxxxxxxxxxx
      action_type: move
  action:
    - service: light.turn_on
      entity_id: light.gateway_light_28xxxxxxxxxx
      data:
        color_name: "gold"
- alias: Cube event tap_twice
  trigger:
    platform: event
    event_type: cube_action
    event_data:
      entity_id: binary_sensor.cube_15xxxxxxxxxxxx
      action_type: tap_twice
  action:
    - service: light.turn_on
      entity_id: light.gateway_light_28xxxxxxxxxx
      data:
        color_name: "deepskyblue"
- alias: Cube event shake_air
  trigger:
    platform: event
    event_type: cube_action
    event_data:
      entity_id: binary_sensor.cube_15xxxxxxxxxxxx
      action_type: shake_air
  action:
    - service: light.turn_on
      entity_id: light.gateway_light_28xxxxxxxxxx
      data:
        color_name: "blue"
```

#### {% linkable_title Aqara Wireless Switch %}

绿米无线开关有分单键和双键版本。和小米无线开关原理一致，但仅支持单击操作。双键版本会增加一个名为`binary_sensor.wall_switch_both_158xxxxxxxxx12` 的组件，同时增加支持双键同时按下的新事件`both`。

```yaml
- alias: Decrease brightness of the gateway light #调低网关灯的亮度
  trigger:
    platform: event
    event_type: click
    event_data:
      entity_id: binary_sensor.wall_switch_left_158xxxxxxxxx12
      click_type: single
  action:
    service: light.turn_on
    entity_id: light.gateway_light_34xxxxxxxx13
    data_template:
      brightness: {% raw %}>-
        {% if states.light.gateway_light_34xxxxxxxx13.attributes.brightness %}
          {% if states.light.gateway_light_34xxxxxxxx13.attributes.brightness - 60 >= 10 %}
            {{states.light.gateway_light_34xxxxxxxx13.attributes.brightness - 60}}
          {% else %}
            {{states.light.gateway_light_34xxxxxxxx13.attributes.brightness}}
          {% endif %}
        {% else %}
          10
        {% endif %}{% endraw %}

- alias: Increase brightness of the gateway light #调亮网关灯
  trigger:
    platform: event
    event_type: click
    event_data:
      entity_id: binary_sensor.wall_switch_right_158xxxxxxxxx12
      click_type: single
  action:
    service: light.turn_on
    entity_id: light.gateway_light_34xxxxxxxx13
    data_template:
      brightness: {% raw %}>-
        {% if states.light.gateway_light_34xxxxxxxx13.attributes.brightness %}
          {% if states.light.gateway_light_34xxxxxxxx13.attributes.brightness + 60 <= 255 %}
            {{states.light.gateway_light_34xxxxxxxx13.attributes.brightness + 60}}
          {% else %}
            {{states.light.gateway_light_34xxxxxxxx13.attributes.brightness}}
          {% endif %}
        {% else %}
          10
        {% endif %}{% endraw %}

- alias: Turn off the gateway light
  trigger:
    platform: event
    event_type: click
    event_data:
      entity_id: binary_sensor.wall_switch_both_158xxxxxxxxx12
      click_type: both
  action:
    service: light.turn_off
    entity_id: light.gateway_light_34xxxxxxxx13
```


