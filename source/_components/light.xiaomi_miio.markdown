---
layout: page
title: "Xiaomi Philips Light"
description: "Instructions how to integrate your Xiaomi Philips Lights within Home Assistant."
date: 2017-08-26 08:45
sidebar: true
comments: false
sharing: true
footer: true
logo: philips.png
ha_category: Light
ha_version: 0.53
ha_iot_class: "Local Polling"
---

小米 WIFI 设备平台 `xiaomi_miio` 能让你在 HA 中接入米家飞利浦智睿系列灯具，包括吸顶灯和球泡灯。

目前支持的操作有开 `on`，关 `off`，色温设置 `set_cct` 以及亮度设置 `set_bright`。

请先按照 [此教程](/components/vacuum.xiaomi/#retrieving-the-access-token) 获取设备 `token`，之后在 `configuration.yaml` 文件中添加以下配置：

```yaml
light:
  - platform: xiaomi_miio
    name: Xiaomi Philips Smart LED Ball
    host: 192.168.130.67
    token: YOUR_TOKEN
```

变量说明：
- **host** (*必填*): 灯具的 ip
- **token** (*必填*): 灯具的 token
- **name** (*可选*):  灯具的昵称


