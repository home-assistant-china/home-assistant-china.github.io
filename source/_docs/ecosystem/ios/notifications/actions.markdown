---
layout: page
title: "Actionable notifications"
description: "Making push notifications a two way system"
date: 2016-10-25 15:00:00 -0700
sidebar: true
comments: false
sharing: true
footer: true
redirect_from: /ecosystem/ios/notifications/actions/
---

交互式通知允许您将1-4个自定义按钮附加到通知中。 当选择其中一个动作时，Home Assistant将会被通知哪个动作被选择。 这允许您构建更复杂的自动化场景。

交互式通知示例：

- 当您在外出或者入睡后，如果在家中检测到了物体移动，手机将会接收到通知。 您可以添加一个动作（action）去触发警报。 当按下时，Home Assistant会被通知`响起警报(Sound Alarm)`动作被选中。 每当该事件被触发时，你可以添加一个自动化场景来响起防盗报警。
- 当有人按你家的门铃时。 您可以发送一个动作来锁定或解锁您的屋门。 点击时，通知将发送到Home Assistant您可以以此来构建自动化场景。
- 每当您的车库门打开时您可以发送一个通知，与此同时提供打开和关闭车库门的动作。

<p class='img'>
  <img src='/images/ios/actions.png' />
  交互式通知允许用户将命令发送回Home Assistant。
</p>

## {% linkable_title 交互式通知如何工作的概述 %}

在发送通知之前：

1. 在Home Assistant配置中需要定义一个包含1-4个操作的通知类别。
2. 当iOS应用启动时，Home Assistant会请求开启推送通知（也可以在通知设置中手动启用）。

当发送通知时：

1. 发送一个通知并将其中的`data.push.category`设置为一个预定的通知类别标识
2. 推送通知被发送到设备
3. 用户打开推送通知
3. 用户点击相关动作
4. 该操作标识以事件`ios.notification_action_fired`中的`actionName`属性与其他元数据（如设备和类别名称）被一起发送回HA。

<p class='img'>
  <img src='/images/ios/NotificationActionFlow.png' />
  iOS设备和Home Assistant协同工作实现交互式通知示例。
</p>

## {% linkable_title 定义 %}
- 类别 - 类别表示应用可能收到的通知类型。 其可以被当做是一组独特的动作。 类别参数包括：
- 动作 - 动作包含按钮标题以及iOS在选择动作时用以通知应用程序时所需的信息。 您可以为应用程序创建单独的操作对象以此来实现对不同操作的支持。 动作参数包括：

## {% linkable_title 类别参数 %}

- **name** (*必选*): 这个类别的易读名称。
- **identifier** (*必选*): 该类别的唯一标识符。 必须是大写字母且没有特殊字符或空格。
- **action** (*必选*): 动作清单

## {% linkable_title 动作参数 %}

- **identifier** (*必选*): 此动作的唯一标识符。 必须是大写字母且没有特殊字符或空格。只需要对该类别是唯一的，而不需要是全局唯一。
- **title** (*必选*): 按钮上显示的文字。尽可能短一些。
- **activationMode** (*可选*): 在动作被执行时，应用程序的运行模式。若设置为`foreground`将使应用程序在选择后打开。 默认值为`background`。
- **authenticationRequired** (*可选*): 如果布尔类型（`true`，`True`，`yes`等），则在执行操作之前，用户必须解锁设备。
- **destructive** (*可选*): 当该属性的值为布尔值时，系统将以不同的方式显示相应的按钮，以指示操作是需要注意的（其文本颜色为红色）。
- **behavior** (*可选*): 当使用`textInput`时，系统为用户提供了一种在通知中的输入文本的回应方式。 输入的文字将被发送到Home Assistant。 默认值为`default`。
- **textInputButtonTitle** (*可选*): 按钮标签。如果`behavior`设置为`textInput`则该参数为 *必选*
- **textInputPlaceholder** (*可选*): 文本输入框中显示的占位符文本。 仅当`behavior`为`textInput`，且设备运行iOS 10时才可使用。

以下是一个包括所有参数的配置示例：

```yaml
ios:
  push:
    categories:
      - name: Alarm
        identifier: 'ALARM'
        actions:
          - identifier: 'SOUND_ALARM'
            title: 'Sound Alarm'
            activationMode: 'background'
            authenticationRequired: yes
            destructive: yes
            behavior: 'default'
          - identifier: 'SILENCE_ALARM'
            title: 'Silence Alarm'
            activationMode: 'background'
            authenticationRequired: yes
            destructive: no
            behavior: 'textInput'
            textInputButtonTitle: 'Silencio!'
            textInputPlaceholder: 'Placeholder'
```

## {% linkable_title 构建通知操作的自动化 %}
以下是一个发送在数据体（payload）中包含类别（category）的通知的自动化示例：

```yaml
automation:
  - alias: Notify iOS app
    trigger:
      ...
    action:
      service: notify.ios_robbies_iphone_7_plus
      data:
        message: "Something happened at home!"
        data:
          push:
            badge: 5
            sound: <SOUND FILE HERE>
            category: "ALARM" # 需要匹配在ios配置中所使用的顶级标识符
          action_data: # 在action_data中传递的任何内容都会被回传给Home Assistant。
            entity_id: light.test
            my_custom_data: foo_bar
```

当选择一个动作时，一个名为`ios.notification action fired`的事件将在Home Assistant事件总线（event bus）上被触发。 下面是一个数据体示例。

```json
{
  "sourceDeviceName": "Robbie's iPhone 7 Plus",
  "sourceDeviceID": "robbies_iphone_7_plus",
  "actionName": "SOUND_ALARM",
  "sourceDevicePushId": "ab9f02fe-6ac6-47b8-adeb-5dd87b489156",
  "textInput": "",
  "actionData": {}
}
```

以下是对于给定数据体的自动化示例：

```yaml
automation:
  - alias: Sound the alarm
    trigger:
      platform: event
      event_type: ios.notification_action_fired
      event_data:
        actionName: SOUND_ALARM
    action:
      ...
```

注释：

* 只有当`behavior`设置为`textInput`时，`textInput`才会存在。
* `actionData`是一个字典，其包括原始通知中`push`字典中`action_data`字典中所用的传递参数
