---
layout: page
title: "Requesting location updates"
description: "Ask the device to send a location update remotely"
date: 2016-10-25 15:00:00 -0700
sidebar: true
comments: false
sharing: true
footer: true
redirect_from: /ecosystem/ios/notifications/requesting_location_updates/
---

<p class="note warning">
**由于下述时间限制，请不要依赖此功能。**
</p>

您可以通过发送特殊通知来强制设备尝试报告其位置。

```yaml
automation
  - alias: Notify iOS app
    trigger:
      ...
    action:
      service: notify.ios_<your_device_id_here>
      data:
        message: "request_location_update"
```

假设设备收到通知，它将尝试在5秒内获取位置更新并将其报告给Home Assistant。这个有点看运气，因为苹果加强了对应用程序处理通知所用最大时间的限制，而位置更新受诸多因素影响，有时会花费很长时间获取位置，比如在等待GPS信号。
