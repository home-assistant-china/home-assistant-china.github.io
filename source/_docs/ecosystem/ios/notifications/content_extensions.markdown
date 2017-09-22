---
layout: page
title: "Dynamic content"
description: "Extend your notifications with dynamic content"
date: 2016-10-25 15:00:00 -0700
sidebar: true
comments: false
sharing: true
footer: true
redirect_from: /ecosystem/ios/notifications/content_extensions/
---

使用iOS 10中新的内容扩展功能，动态内容现在可以在不打开应用的情况下显示为通知的一部分。

# 地图
地图将在给定的坐标处显示带有红色大头针。
地图将以给定的坐标为中心。

```yaml
service: notify.ios_<your_device_id_here>
data:
  message: Something happened at home!
  data:
    push:
      category: map
    action_data:
      latitude: "40.785091"
      longitude: "-73.968285"
```

## 显示第二个大头针

您可以使用 `action_data` 的以下属性来显示第二个大头针。 如果使用，第一个大头针为红色，第二个为绿色。

- **second_latitude**: 第二个大头针的纬度。 **必须是字符串！**
- **second_longitude**: 第二个大头针的经度。 **必须是字符串！**
- **shows_line_between_points**: 布尔值，用以指示是否应在第一个和第二个大头针之间绘制一条线。

## 额外配置

您还可以通过修改 `action_data` 中以下属性来修改地图。 除非另有说明，否则所有值都将为布尔值：

- **shows_compass**: 布尔值，地图是否显示罗盘。
- **shows_points_of_interest**: 布尔值，地图是否显示兴趣点信息。
- **shows_scale**: 布尔值，地图是否显示比例信息。
- **shows_traffic**: 布尔值，地图是否显示交通信息。
- **shows_user_location**: 布尔值，地图是否应尝试显示用户位置。

<p class='img'>
  <img src='/images/ios/map.png' />
  动态地图。
</p>


# 摄像头直播

通知的缩略图为来自摄像头的静态图像。通知内容是摄像头的实时MJPEG流（假设摄像头支持）。

您可以使用附件参数中的`content-type`和`hide-thumbnail`对摄像头进行设置。

演示视频在[这里](https://www.youtube.com/watch?v=LmYwpxPKW0g).

```yaml
service: notify.ios_<your_device_id_here>
data:
  message: Motion detected in the Living Room
  data:
    push:
      category: camera
    entity_id: camera.demo_camera
```

<div class='videoWrapper'>
<iframe width="560" height="315" src="https://www.youtube.com/embed/LmYwpxPKW0g" frameborder="0" allowfullscreen></iframe>
</div>

# 结合交互式通知

如上所示，`category`键是用来告诉设备要使用什么类型的扩展内容。您可以在自定义的[交互式通知](/ecosystem/ios/notifications/actions/)中使用相同类别的标识符，用以向扩展内容中添加操作。

# 故障排除

 如果您在接收这些特殊通知时遇到问题，请先尝试重启手机。扩展拆件有时在重新启动之前可能无法正确加载。
