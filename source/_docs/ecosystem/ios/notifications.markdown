---
layout: page
title: "Notifications Introduction"
description: "Getting started with iOS notifications"
date: 2016-10-25 15:00:00 -0700
sidebar: true
comments: false
sharing: true
footer: true
redirect_from: /ecosystem/ios/notifications/
---

'ios'通知平台可以将推送通知发送到Home Assistant的iOS应用。

'ios'组件将自动加载通知服务。
您可以使用`service：notify.ios_ <your_device_ID>`调用该服务组件。
您的设备ID可以在配置文件夹中的`ios.conf`文件中找到。 该文件为压缩JSON格式。 您可以通过复制文件内容并将其粘贴到[JSONLint](http://jsonlint.com)使其更加易读。

在该例子中，设备的ID是`robbiet480_7plus`，所以要用`notify.ios_robbiet480_7plus`：来调用通知服务：
```json
{"devices":{"robbiet480_7plus":{"app":{"bundleIdentifer":"io.robbie.HomeAssistant","versionNumber":1,"buildNumber":53},"pushSounds":[],"permissions":["location"],"deviceId":"robbiet480_7plus","device":{"type":"iPhone 7 Plus","systemName":"iOS","systemVersion":"10.3","permanentID":"AB9F02FE-6AC6-47B8-ADEB-5DD87B489156","localizedModel":"iPhone","name":"Robbie's iPhone 7 Plus","model":"iPhone"},"battery":{"state":"Full","level":100},"pushToken":"SECRET","pushId":"SECRET"}}}
```

您可以在[基本通知](https://home-assistant.io/docs/ecosystem/ios/notifications/basic/)文档和[交互式通知](https://home-assistant.io/docs/ecosystem/ios/notifications/actions/)文档中找到更多的相关内容。
