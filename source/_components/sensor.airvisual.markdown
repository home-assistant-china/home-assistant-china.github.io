---
layout: page
title: "AirVisual"
description: "Instructions on how to use AirVisual data within Home Assistant"
date: 2017-09-06 12:15
sidebar: true
comments: false
sharing: true
footer: true
logo: airvisual.jpg
ha_category: Health
ha_release: 0.53
ha_iot_class: "Cloud Polling"
---

`airvisual`传感器平台通过使用 [AirVisual](https://airvisual.com/) 的 API 向服务器获取制定经纬度地点或城市的空气质量数据。该平台支持的指数有：空气质量指数 (AQI)、空气质量等级及主要污染物。数据可选择使用[美国标准和/或中国标准](http://www.clm.com/publication.cfm?ID=366)。

使用该平台要求已有 AirVisual API key，可知[官网](https://airvisual.com/api)获取。 注意：申请时请选择 "Community" 类型，"Startup" 和 "Enterprise" 类的 key 可能在本平台无法工作。

<p class='note warning'>
"Community" API key 每月限制调用 10,000 次，因此本平台将调用时间设定为每 10 分钟 1 次以配合此限制。</p>

## {% linkable_title Configuring the Platform via Latitude/Longitude %}

使用本平台并通过纬度/经度收集数据，将下列命令增添至 `configuration.yaml` 文件：

```yaml
sensor:
  - platform: airvisual
    api_key: abc123
    monitored_conditions:
      - us
      - cn
    latitude: 42.81212
    longitude: 108.12422
    radius: 500
```

变量说明：

- **api_key** (*必须*): 你的 AirVisual API key
- **monitored_conditions** (*必须*): 数据标准
(`us` 为美国， `cn` 为中国)
- **latitude** (*可选*): 监测地区纬度；如果没有设定，默认为 `configuration.yaml` 中所设的系统纬度
- **longitude** (*可选*): 监测地区经度；如果没有设定，默认为 `configuration.yaml` 中所设的系统经度
- **radius** (*可选*): 监测范围，即监测距离所涉中心点多大范围内城市的数据；默认为 `1000`；单位默认为米


## {% linkable_title Configuring the Platform via City/State/Country %}

To enable the platform and gather data via city/state/country, add the
following lines to your `configuration.yaml` file:

```yaml
sensor:
  - platform: airvisual
    api_key: abc123
    monitored_conditions:
      - us
      - cn
    city: southend-on-sea
    state: essex
    country: uk
```

Configuration variables:

- **api_key** (*Required*): your AirVisual API key
- **monitored_conditions** (*Required*): the air quality standard(s) to use
(`us` for U.S., `cn` for Chinese)
- **city** (*Optional*): the city to monitor
- **state** (*Optional*): the state/region to monitor
- **country** (*Optional*): the country to monitor

To easily determine the proper values for a particular location, use the
[AirVisual region directory](https://airvisual.com/world). Once you browse to the particular city you want,
take note of the breadcrumb title, which is of the form
`country > state/region > city`. Use this information to fill out
`configuration.yaml`.

For example, Sao Paulo, Brazil shows a breadcrumb title of
`Brazil > Sao Paulo > Sao Paulo` – thus, the proper configuration would look
like this:

```yaml
sensor:
  - platform: airvisual
    api_key: abc123
    monitored_conditions:
      - us
      - cn
    city: sao-paulo
    state: sao-paulo
    country: brazil
```

## {% linkable_title Sensor Types %}

平台接入后，将会生成 3 个传感器显示所监测的数据，包括：

### 空气质量指数

**描述：** 该传感器显示 AQI 数值

**示例传感器 ID：** `sensor.chinese_air_quality_index`

**示例传感器值：** `32`

**解释：**

AQI | 等级 | 描述
------- | :----------------: | ----------
0 - 50  | **优秀** | 空气质量令人满意，基本无空气污染，各类人群可正常活动。
51 - 100  | **良** | 空气质量可接受，但某些污染物可能对极少数异常敏感人群健康有较弱影响，建议极少数异常敏感人群应减少户外活动。
101 - 150 | **轻度污染** | 易感人群症状有轻度加剧，健康人群出现刺激症状。建议儿童、老年人及心脏病、呼吸系统疾病患者应减少长时间、高强度的户外锻炼。
151 - 200 | **中度污染** | 进一步加剧易感人群症状，可能对健康人群心脏、呼吸系统有影响，建议疾病患者避免长时间、高强度的户外锻练，一般人群适量减少户外运动。
201 - 300 | **重度污染** | 心脏病和肺病患者症状显著加剧，运动耐受力降低，健康人群普遍出现症状，建议儿童、老年人和心脏病、肺病患者应停留在室内，停止户外运动，一般人群减少户外运动。
301+ | **有毒害** | 健康人群运动耐受力降低，有明显强烈症状，提前出现某些疾病，建议儿童、老年人和病人应当留在室内，避免体力消耗，一般人群应避免户外活动。

### 空气污染等级

**描述：** 该传感器显示空气污染等级

**示例传感器 ID：** `sensor.us_air_pollution_level`

**示例传感器值：** `Moderate`

### 主要污染物

**描述：** 该传感器显示主要污染物

**示例传感器 ID：** `sensor.us_main_pollutant`

**示例传感器值：** `PM2.5`

**Explanation:**

污染物 | 符号 | 更多信息
------- | :----------------: | ----------
可入肺颗粒物 (<= 2.5 μm) | PM2.5 | [EPA: Particulate Matter (PM) Pollution ](https://www.epa.gov/pm-pollution)
可吸入颗粒物 (<= 10 μm) | PM10 | [EPA: Particulate Matter (PM) Pollution ](https://www.epa.gov/pm-pollution)
臭氧 | O | [EPA: Ozone Pollution](https://www.epa.gov/ozone-pollution)
二氧化硫 | SO2 | [EPA: Sulfur Dioxide (SO2) Pollution](https://www.epa.gov/so2-pollution)
一氧化碳 | CO | [EPA: Carbon Monoxide (CO) Pollution in Outdoor Air](https://www.epa.gov/co-pollution)


