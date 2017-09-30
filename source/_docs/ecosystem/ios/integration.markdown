---
layout: page
title: "Integration"
description: "Examples of how Home Assistant for iOS can be integrated with other apps"
date: 2016-10-25 15:00:00 -0700
sidebar: true
comments: false
sharing: true
footer: true
redirect_from: /ecosystem/ios/integration/
---

Home Assistant通过URL打开其他应用程序已实现对iOS设备的支持。

调用时查询参数以字典的方式被传入。

## 调用服务
示例: `homeassistant://call_service/device_tracker.see?entity_id=device_tracker.entity`

## 触发事件
您可以创建一个[事件触发器](https://home-assistant.io/docs/automation/trigger/#event-trigger) 然后触发该事件

示例: `homeassistant://fire_event/custom_event?entity_id=MY_CUSTOM_EVENT`

## 发送位置
示例: `homeassistant://send_location/`
