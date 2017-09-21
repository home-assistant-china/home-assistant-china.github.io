---
layout: page
title: "Basic Notifications"
description: "Basic notes about iOS notifications"
date: 2016-10-25 15:00:00 -0700
sidebar: true
comments: false
sharing: true
footer: true
redirect_from: /ecosystem/ios/notifications/basic/
---

iOS通知平台接受标准的 `title`, `message` 和 `target` 参数。 iOS通知平台以服务的方式对目标设备提供支持。 假设您在配置平台时没有设置`name`，那么您应该可以看到所有已注册和启用通知服务的iOS设备，这些设备都可以服务的方式接受通知。其服务名称的前缀为“notify.ios_”，后面则为设置时所输入的设备名称。

注释：

* `title` 仅在Apple Watch和iOS 10设备上显示。

* `target` 可用在 `ios.conf` 中的PushID来制定使用某个特定设备。提供目标的首选方式是通过指定特定目标的通知服务。

<p class='img'>
  <img src='/images/ios/example.png' />
  推送通知示例，其显示了包含`主标题（title）`，`副标题（subtitle）`，`信息（message）`和 [交互式通知](/ecosystem/ios/notifications/actions/)在内的所有基本选项。
</p>

### {% linkable_title 优化基本通知 %}

#### 徽章
您可以在数据体（payload）中设置徽章图标：

```yaml
automation:
  - alias: Notify iOS app
    trigger:
      ...
    action:
      service: notify.ios_<your_device_id_here>
      data:
        message: "Something happened at home!"
        data:
          push:
            badge: 5
```

#### 副标题
除主标题外，iOS 10还支持副标题：

```yaml
automation
  - alias: Notify iOS app
    trigger:
      ...
    action:
      service: notify.ios_<your_device_id_here>
      data:
        message: "Something happened at home!"
        data:
          subtitle: "Subtitle goes here"
```

### {% linkable_title 向多个手机发送通知 %}
要向多个手机发送通知，请创建一个 [群组通知](https://home-assistant.io/components/notify.group/):
```yaml
notify:
  - name: NOTIFIER_NAME
    platform: group
    services:
      - service: ios_iphone_one
      - service: ios_iphone_two
```
现在，您可以使用以下方式向群组中的所有人发送通知：
```yaml
  automation:
    - alias: Notify iOS app
      trigger:
        ...
      action:
        service: notify.NOTIFIER_NAME
        data:
          message: "Something happened at home!"
          data:
            push:
              badge: 5
```
