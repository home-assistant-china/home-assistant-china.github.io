---
layout: page
title: "iOS"
description: "Documentation about the Home Assistant iOS app."
release_date: 2016-10-24 15:00:00 -0700
sidebar: true
comments: false
sharing: true
footer: true
redirect_from: /ecosystem/ios/
---

Home Assistant iOS应用为iOS提供了一个配套的应用程序。它深度融合了Home Assistant和iOS。 其基本功能包括：

* 高级推送通知
* 位置追踪
* 所有Home Assistant实体的基本控制
* 第三方应用程序集成

在苹果支持的各个国家/地区的iOS应用商店里，均可下载该应用程序。

<p style="text-align: center;"><a target="_blank" href="https://itunes.apple.com/us/app/home-assistant-open-source-home-automation/id1099568401?mt=8" style="display:inline-block;overflow:hidden;background:url(//linkmaker.itunes.apple.com/assets/shared/badges/en-us/appstore-lrg.svg) no-repeat;width:135px;height:40px;background-size:contain;"></a></p>

## 基本要求

* iOS设备至少运行iOS 9，但iOS 10更好。
* Home Assistant 0.42.4或者以上，用以推送通知支持。
* 强烈建议使用SSL。由于苹果的限制，自签名的SSL证书将无法正常工作。

`ios`组件是Home Assistant iOS应用的配套组件。虽然不是必须的，但是将`ios`组件添加到您的设置中将大大增强iOS应用程序的功能，其包括新的推送通知，以及不能被独立应用程序所使用的位置和传感器功能。

加载`ios`组件会同时加载： [`device_tracker`][device-tracker], [`zeroconf`][zeroconf] 和 [`notify`][notify] 平台.

## {% linkable_title 设置 %}

### 自动设置

在以下情况下，`ios`组件将会被自动加载：

1. [`discovery`][discovery]组件被启用
2. 您刚刚安装了应用程序，且刚开始运行。

自动发现和组件加载仅在首次安装应用程序时才会发生。因此您可能需要等待几分钟才能加载iOS组件，因为`discovery`组件只有每5分钟的时候才会扫描一次网络。

第一次自动设置后，您需要在配置文件中添加`ios:`，以便以后重启Home Assistant后，该组件也会被默认加载。

### 手动设置

您还可以通过在配置文件中添加以下内容来手动加载`ios`组件：

```yaml
# configuration.yaml接入示例
ios:
```

可配置变量:

- **push** (*可选*): 交互式推送通知配置。详情见[交互式通知文件][actionable-notifications]。

[discovery]: /components/discovery
[device-tracker]: /components/device_tracker
[zeroconf]: /components/zeroconf
[notify]: /components/notify
[actionable-notifications]: /docs/ecosystem/ios/notifications/actions/
