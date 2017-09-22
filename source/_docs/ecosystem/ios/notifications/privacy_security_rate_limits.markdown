---
layout: page
title: "Privacy, rate limiting and security"
description: "Notes about important topics relating to push notifications"
date: 2016-10-25 15:00:00 -0700
sidebar: true
comments: false
sharing: true
footer: true
redirect_from: /ecosystem/ios/notifications/privacy_security_rate_limits/
---

## {% linkable_title 隐私 %}

远程服务器上不会存储通知内容。只有所需的推送注册数据和每台设备每天发送的推送通知的总数（用于数量限制）被保存。

## {% linkable_title 数量限制 %}

目前，每台设备每天最多可以发送150个推送通知。这是为了确保服务维护的低成本。将来我们可能会对此进行升级，以支持允许更多的通知。 数量限制在UTC时间的每天午夜重置。发送通知后，您当前数量限制信息（包括当天已发通知和剩余可发送通知）将被输出到您的Home Assistant日志。如果发送通知时发生了错误，将不会计入您的数量限制中。

## {% linkable_title 安全 %}

所有Home Assistant，推送服务器和Apple之间的数据都使用SSL加密。
