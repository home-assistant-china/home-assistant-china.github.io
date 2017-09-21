---
layout: page
title: "Notification attachments"
description: "Adding attachments to iOS push notifications"
date: 2016-10-25 15:00:00 -0700
sidebar: true
comments: false
sharing: true
footer: true
redirect_from: /ecosystem/ios/notifications/attachments/
---

iOS 10在通知中添加了 _附件_。 附件是当收到通知，下载到设备的图像，视频或音频文件，其将在通知旁边显示。 当通知未展开时，将显示其缩略图。 当扩展通知时，将显示全尺寸附件。

<p class="note">
要在3D Touch设备上展开通知，只需重触任何通知。 在非3D触摸设备上则需滑动并点按“查看”按钮。
</p>

```yaml
- alias: Notify iOS app
    trigger:
      ...
    action:
      service: notify.ios_robbies_iphone_7_plus
      data:
        message: "Something happened at home!""
        data:
          attachment:
            url: https://67.media.tumblr.com/ab04c028a5244377a0ab96e73915e584/tumblr_nfn3ztLjxk1tq4of6o1_400.gif
            content-type: gif
            hide-thumbnail: false
```

注释：
* 通知的缩略图为`url`所指向的媒体。
* 通知内容为`url`所包含的媒体。
* 附件可用于自定义推送通知类别。

## 示例

<p class='img'>
  <img src='/images/ios/attachment.png' />
  带有附件的未展开推送通知。
</p>

<p class='img'>
  <img src='/images/ios/expanded_attachment.png' />
  同样的通知，扩展后显示为全尺寸附件
</p>

## 支持的媒体类型

如果附件没有出现，请确保它属于以下格式之一：

### 音频附件

最大文件尺寸: 5 MB

支持格式: AIFF, WAV, MP3, MPEG4 Audio

### 图片附件

最大文件尺寸: 10 MB

支持格式: JPEG, GIF, PNG

### 视频附件

最大文件尺寸: 50 MB

支持格式: MPEG, MPEG2, MPEG4, AVI

## 配置

- **url** (*必选*): 要用作附件内容的URL。 该URL _必须_ 可以从互联网访问，或者接收设备必须与托管内容位于同一网络上。
- **content-type** (*可选*): 默认情况下，URL的后缀名将被检查以确定文件类型。 如果没有后缀/无法确定，您可以手动提供文件后缀名。
- **hide-thumbnail** (*可选*): 如果设置为 `true`，则缩略图将不会被显示在通知上。其内容只能展开后才能看到。
