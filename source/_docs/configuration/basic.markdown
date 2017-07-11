---
layout: page
title: "Setup basic information"
description: "Setting up the basic info of Home Assistant."
date: 2015-03-23 12:50
sidebar: true
comments: false
sharing: true
footer: true
redirect_from: /getting-started/basic/
---

在默认情况下，HomeAssistant将试图通过你的IP地址来检测你的地理位置，并将基于你的地理位置，自动选择温度单位和时区。当然你也可以覆盖 `configuration.yaml` 中的以下内容，来指明你的位置信息:

```yaml
homeassistant:
  # 若非指明，此部分的缺省数据将通过freegeoip.io来自动获取

  # 经度和纬度数据，用来计算日出和日落时间
  latitude: 32.87336
  longitude: 117.22743

  # 影响天气和日出日落数据（海拔高度，单位：米）
  elevation: 430

  # 公制单位为：'metric'，英制单位为：'imperial'
  unit_system: metric

  # 参考以下链接来选择你的时区:
  # http://en.wikipedia.org/wiki/List_of_tz_database_time_zones
  time_zone: America/Los_Angeles

  # 运行HomeAssistant系统的地点，可自定义
  name: Home
```

### {% linkable_title Password protecting the web interface %}

首先，你需要为HomeAssistant的Web界面设置一个密码。使用你最喜欢的文本编辑工具打开 `configuration.yaml` 并且编辑其中的 `http` 部分:

```yaml
http:
  api_password: 你的密码
```

<p class='note warning'>
如果你决定要暴露你的HomeAssistant界面到公网，并且忘记设置密码，那么所有人都将能访问你的HomeAssistant。
</p>

更多选项如HTTPS加密，请访问 [HTTP组件文档](/components/http/)。
