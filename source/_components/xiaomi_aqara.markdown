---
layout: page
title: "Xiaomi Gateway"
description: "Instructions how to integrate your Xiaomi Gateway within Home Assistant."
date: 2017-07-21 16:34
sidebar: true
comments: false
sharing: true
footer: true
logo: xiaomi.png
ha_category: Hub
ha_release: "0.50"
ha_iot_class: "Local Push"
---


小米平台 `xiaomi aqara` 允许你在 HA 中接入[小米](http://www.mi.com/en/)的 Zigbee 智能家居设备。支持的设备包括：

- 温度湿度传感器（新旧版）
- 人体运动传感器 （新旧版）
- 门窗传感器 （新旧版）
- 无线开关 （新旧版）
- 智能插座 Zigbee 版本 （反馈耗电、电力负载、通电、开关状态）
- 墙壁开关 (反馈耗电、电力负载、 通电状态）
- 绿米（Aqara）全系列墙壁开关，包括单、双键；单、零火；
- 绿米 （Aqara）无线开关单、双键
- 魔方控制器
- 天然气泄漏传感器 (反馈警报和浓度信息）
- 烟雾报警器 (反馈警报和浓度信息)
- 多功能网关 (支持网关灯、亮度传感器及铃声播放)
- 绿米智能窗帘器
- 浸水传感器
- 各设备电量

不支持：

- 网关广播
- 网关按键
- 绿米及米家空调伴侣
- 墙壁开关独立使用状态
- 烟雾及天然气报警器的其他警报事件，包括：模拟警报、电池错误警报、灵敏度警报及 I2C 通信失败警报。


Follow the setup process using your phone and Mi-Home app. From here you will be able to retrieve the key from within the app following [this tutorial](https://community.home-assistant.io/t/beta-xiaomi-gateway-integration/8213/1832)

### {% linkable_单个网关 %}
请查看该[章节](/xiaomi/#retrieving-the-access-token)以获取API token。

（译者注：请在米家 app 中与网关配对，之后进入网关页，点选右上角“……” —— 关于 —— 空白处点多下 —— 局域网通信协议 —— 打开并获取密码）

之后在 `configuration.yaml` 文件中填入如下配置：

```yaml
# 使用单网关前提下，可不填 mac
xiaomi_aqara:
  gateways:
   - mac:
     key: xxxxxxxxxxxxxxxx
```


### {% linkable_title 多个网关 %}

```yaml
# 多个网关必须填入 mac
xiaomi_aqara:
  gateways:
    - mac: xxxxxxxxxxxx
      key: xxxxxxxxxxxxxxxx
    - mac: xxxxxxxxxxxx
      key: xxxxxxxxxxxxxxxx
```


### {% linkable_title 搜寻固定 IP 的网关 %}

```yaml
# 12 字符 MAC地址可以从网关获取
xiaomi_aqara:
  interface: '192.168.0.1'
  gateways:
    - mac: xxxxxxxxxxxx
      key: xxxxxxxxxxxxxxxx
```

变量说明：

- **mac** (*可选*): 网关 mac 地址，使用多个网关则必须填写
- **key** (*可选*): 网关通信协议密码。如果想要控制网关灯和开关，则必须填写；传感器在无密码情况下仍可正常运作。
- **discovery_retry** (*可选*): 连接失败重试次数，默认为 3。
- **interface** (*可选*): 所使用的接口，默认为全部(all）。

## {% linkable_title Services %}

HA 支持网关铃声的两个操作：播放 `xiaomi.play_ringtone` 和停止`xiaomi.stop_ringtone`。网关铃声的播放需要 `1.4.1_145` 及以上网关固件支持。 同时必须提供铃声 ID `ringtone_id`  和网关 Mac `gw_mac`。 铃声音量控制 `ringtone_vol` 是可选的。全部铃声 ID`ringtone_id`  有：

- alarm ringtones [0-8]
- doorbell ring [10-13]
- alarm clock [20-29]
- custom ringtones (uploaded by the Mi Home app) starting from 10001

（译者注：中文对应名称大家可前往米家 app 中查询）

自动化示例

```yaml
- alias: Let a dog bark on long press #长按开关后网关发出狗叫
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

- alias: Stop barking immediately on single click
  trigger:
    platform: event
    event_type: click
    event_data:
      entity_id: binary_sensor.switch_158d000xxxxxc2
      click_type: single
  action:
    service: xiaomi_aqara.stop_ringtone
    data:
      gw_mac: xxxxxxxxxxxx
```

### {% linkable_title Troubleshooting %}

**Connection problem**

```bash
2017-08-20 16:51:19 ERROR (SyncWorker_0) [homeassistant.components.xiaomi] No gateway discovered
2017-08-20 16:51:20 ERROR (MainThread) [homeassistant.setup] Setup failed for xiaomi: Component failed to initialize.
```

That means that Home Assistant is not getting any response from your Xiaomi gateway. Might be a local network problem or your firewall.
- Make sure you have enabled LAN access: https://community.home-assistant.io/t/beta-xiaomi-gateway-integration/8213/1832
- Turn off the firewall on the system where Home Assistant is running.
- Try to leave the MAC address `mac:` blank. 
- Try to set `discovery_retry: 10`.
- Try to disable and then enable LAN access.
- Hard reset the gateway: Press the button of the gateway 30 seconds and start again from scratch.



