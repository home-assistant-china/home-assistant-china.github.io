---
layout: page
title: "Location"
description: "Documentation about the location tracking abilities in Home Assistant for iOS"
date: 2016-10-25 15:00:00 -0700
sidebar: true
comments: false
sharing: true
footer: true
redirect_from: /ecosystem/ios/location/
---

## {% linkable_title Home Assistant区外的位置跟踪 %}

Home Assistant ios应用从ios接受 _重要位置更新_。每当收到更新时，更新将会被发送到Home Assistant。一般为，每次你的设备切换到新的基站时，过去了很长一段时间(一般为几个小时)时或者设备的链接状态改变与此同时系统注意到你的位置近期发生了变化时。

苹果对于重要位置更新的[定义][apple-location-programming-guide]为:

> 只有在设备位置发生重大变化（例如500米以上）时，重要位置更新服务才会提供更新。

他们同时在[能效指南][apple-energy-guide]中指出:

> 即使在没有发生位置变更前提下，重要位置更新服务仍然会每15分钟唤醒系统和您的应用程序一次。

最后，我认为来自[Stack Overflow][stackoverflow]定义更加准确:

> 重要位置更新是所有位置监控类型中最不准确的。 因为只有当有基站发生更改时，它才能获取更新。 这就意味用户在不同地区时会影响其准确度和更新频率。 在市区基站多时更新频率会更快。 而在基站较少的郊区更新频率则会很慢。

到底底什么是重要位置更新? 谁知道呢, 苹果对此一向保密.

## {% linkable_title Home Assistant区内的位置跟踪 %}

当Home AssistantiOS应用运行时, 它将在Home Assistant配置中为所有区域设置地理围栏。 当出入该区域时，home assistant将会收到通知。nt.

### 配置

将`track_ios: false`添加到您的区域配置中，以禁用所有已连接iOS应用的区域位置跟踪。

### iBeacons

从1.0.3起，该应用对使用iBeacons来触发进入/退出更新实现了基本支持。 要配置它们，请将您的iBeacon详细信息添加到您的区域，如下所示：

```yaml
zone.home:
  beacon:
    uuid: B9407F30-F5F8-466E-AFF9-25556B57FE6D
    major: 60042
    minor: 43814
```

重启Home Assistant和iOS应用程序。 它将开始使用iBeacons _而不是您的位置信息_ 来触发进入和退出您已定义的区域。 要添加一个iBeacon到 `zone.home`请将以上代码添加到你的`customize`中。

[定义]: https://developer.apple.com/library/content/documentation/Performance/Conceptual/EnergyGuide-iOS/LocationBestPractices.html#//apple_ref/doc/uid/TP40015243-CH24-SW4
[能效指南]: https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/LocationAwarenessPG/CoreLocation/CoreLocation.html#//apple_ref/doc/uid/TP40009497-CH2-SW9
[stackoverflow]: http://stackoverflow.com/a/13331625/486182
