---
layout: page
title: "Notification Sounds"
description: "Adding sounds to notifications"
date: 2016-10-25 15:00:00 -0700
sidebar: true
comments: false
sharing: true
footer: true
redirect_from: /ecosystem/ios/notifications/sounds/
---

为通知添加自定义铃声可让您轻松识别不同通知，而无需查看设备。Home Assistant iOS应用包含了一些预置的通知铃声，但您也可以上传自己的铃声。

以下是一个使用预置铃声的通知示例。

```yaml
- alias: Notify iOS app
  trigger:
    ...
  action:
    service: notify.ios_<your_device_id_here>
    data:
      message: “Something happened at home!”
      data:
        push:
          sound: "US-EN-Morgan-Freeman-Roommate-Is-Arriving.wav"
```

注释：
* 您必须在数据体中使用完整的文件名（包括后缀名）。

## {% linkable_title 自定义推送通知铃声 %}
该应用程序允许您在推送通知中使用自定义铃声。铃声必须符合[苹果的格式要求][sound-requirements]。您可以在通知的数据体中设置铃声文件名。 要添加声音：

1. 将设备连接到电脑，确保iTunes为最新版本。
2. 在iTunes中选择设备。
3. 在左侧选择 "应用"。
4. 下拉后选择 "文件共享"。
5. 选择 HomeAssistant。
6. 拖放处理好的铃声文件。
7. 点击右下角的同步。
8. 同步完成后，断开与电脑的连接。
9. 在iOS设备上打开Home Assistant应用.
10. 选择 设置 -> 通知设置.
11. 选择 "从iTunes导入铃声".

假设您正确地格式化了声音，您现在可以在推送通知中使用它们了。

注释：
* **请注意，由于在iOS 10的bug，在您重启设备之前，你可能无法播放导入的铃声。这个bug应该很快就会被苹果修复。**
* 上传与现有文件同名的文件，将覆盖原始文件。
* 您可以通过检查配置目录中的`ios.conf`文件来查看每个设备上安装的铃声。它们在`pushSounds`中。

### {% linkable_title 预置通知铃声 %}

```
US-EN-Alexa-Back-Door-Opened.wav
US-EN-Alexa-Back-Door-Unlocked.wav
US-EN-Alexa-Basement-Door-Opened.wav
US-EN-Alexa-Basement-Door-Unlocked.wav
US-EN-Alexa-Boyfriend-Is-Arriving.wav
US-EN-Alexa-Daughter-Is-Arriving.wav
US-EN-Alexa-Front-Door-Opened.wav
US-EN-Alexa-Front-Door-Unlocked.wav
US-EN-Alexa-Garage-Door-Opened.wav
US-EN-Alexa-Girlfriend-Is-Arriving.wav
US-EN-Alexa-Good-Morning.wav
US-EN-Alexa-Good-Night.wav
US-EN-Alexa-Husband-Is-Arriving.wav
US-EN-Alexa-Mail-Has-Arrived.wav
US-EN-Alexa-Motion-At-Back-Door.wav
US-EN-Alexa-Motion-At-Front-Door.wav
US-EN-Alexa-Motion-Detected-Generic.wav
US-EN-Alexa-Motion-In-Back-Yard.wav
US-EN-Alexa-Motion-In-Basement.wav
US-EN-Alexa-Motion-In-Front-Yard.wav
US-EN-Alexa-Motion-In-Garage.wav
US-EN-Alexa-Patio-Door-Opened.wav
US-EN-Alexa-Patio-Door-Unlocked.wav
US-EN-Alexa-Smoke-Detected-Generic.wav
US-EN-Alexa-Smoke-Detected-In-Basement.wav
US-EN-Alexa-Smoke-Detected-In-Garage.wav
US-EN-Alexa-Smoke-Detected-In-Kitchen.wav
US-EN-Alexa-Son-Is-Arriving.wav
US-EN-Alexa-Water-Detected-Generic.wav
US-EN-Alexa-Water-Detected-In-Basement.wav
US-EN-Alexa-Water-Detected-In-Garage.wav
US-EN-Alexa-Water-Detected-In-Kitchen.wav
US-EN-Alexa-Welcome-Home.wav
US-EN-Alexa-Wife-Is-Arriving.wav
US-EN-Daisy-Back-Door-Motion.wav
US-EN-Daisy-Back-Door-Open.wav
US-EN-Daisy-Front-Door-Motion.wav
US-EN-Daisy-Front-Door-Open.wav
US-EN-Daisy-Front-Window-Open.wav
US-EN-Daisy-Garage-Door-Open.wav
US-EN-Daisy-Guest-Bath-Leak.wav
US-EN-Daisy-Kitchen-Sink-Leak.wav
US-EN-Daisy-Kitchen-Window-Open.wav
US-EN-Daisy-Laundry-Room-Leak.wav
US-EN-Daisy-Master-Bath-Leak.wav
US-EN-Daisy-Master-Bedroom-Window-Open.wav
US-EN-Daisy-Office-Window-Open.wav
US-EN-Daisy-Refrigerator-Leak.wav
US-EN-Daisy-Water-Heater-Leak.wav
US-EN-Morgan-Freeman-Back-Door-Closed.wav
US-EN-Morgan-Freeman-Back-Door-Locked.wav
US-EN-Morgan-Freeman-Back-Door-Opened.wav
US-EN-Morgan-Freeman-Back-Door-Unlocked.wav
US-EN-Morgan-Freeman-Basement-Door-Closed.wav
US-EN-Morgan-Freeman-Basement-Door-Locked.wav
US-EN-Morgan-Freeman-Basement-Door-Opened.wav
US-EN-Morgan-Freeman-Basement-Door-Unlocked.wav
US-EN-Morgan-Freeman-Boss-Is-Arriving.wav
US-EN-Morgan-Freeman-Boyfriend-Is-Arriving.wav
US-EN-Morgan-Freeman-Cleaning-Supplies-Closet-Opened.wav
US-EN-Morgan-Freeman-Coworker-Is-Arriving.wav
US-EN-Morgan-Freeman-Daughter-Is-Arriving.wav
US-EN-Morgan-Freeman-Friend-Is-Arriving.wav
US-EN-Morgan-Freeman-Front-Door-Closed.wav
US-EN-Morgan-Freeman-Front-Door-Locked.wav
US-EN-Morgan-Freeman-Front-Door-Opened.wav
US-EN-Morgan-Freeman-Front-Door-Unlocked.wav
US-EN-Morgan-Freeman-Garage-Door-Closed.wav
US-EN-Morgan-Freeman-Garage-Door-Opened.wav
US-EN-Morgan-Freeman-Girlfriend-Is-Arriving.wav
US-EN-Morgan-Freeman-Good-Morning.wav
US-EN-Morgan-Freeman-Good-Night.wav
US-EN-Morgan-Freeman-Liquor-Cabinet-Opened.wav
US-EN-Morgan-Freeman-Motion-Detected.wav
US-EN-Morgan-Freeman-Motion-In-Basement.wav
US-EN-Morgan-Freeman-Motion-In-Bedroom.wav
US-EN-Morgan-Freeman-Motion-In-Game-Room.wav
US-EN-Morgan-Freeman-Motion-In-Garage.wav
US-EN-Morgan-Freeman-Motion-In-Kitchen.wav
US-EN-Morgan-Freeman-Motion-In-Living-Room.wav
US-EN-Morgan-Freeman-Motion-In-Theater.wav
US-EN-Morgan-Freeman-Motion-In-Wine-Cellar.wav
US-EN-Morgan-Freeman-Patio-Door-Closed.wav
US-EN-Morgan-Freeman-Patio-Door-Locked.wav
US-EN-Morgan-Freeman-Patio-Door-Opened.wav
US-EN-Morgan-Freeman-Patio-Door-Unlocked.wav
US-EN-Morgan-Freeman-Roommate-Is-Arriving.wav
US-EN-Morgan-Freeman-Searching-For-Car-Keys.wav
US-EN-Morgan-Freeman-Setting-The-Mood.wav
US-EN-Morgan-Freeman-Smartthings-Detected-A-Flood.wav
US-EN-Morgan-Freeman-Smartthings-Detected-Carbon-Monoxide.wav
US-EN-Morgan-Freeman-Smartthings-Detected-Smoke.wav
US-EN-Morgan-Freeman-Smoke-Detected-In-Basement.wav
US-EN-Morgan-Freeman-Smoke-Detected-In-Garage.wav
US-EN-Morgan-Freeman-Smoke-Detected-In-Kitchen.wav
US-EN-Morgan-Freeman-Someone-Is-Arriving.wav
US-EN-Morgan-Freeman-Son-Is-Arriving.wav
US-EN-Morgan-Freeman-Starting-Movie-Mode.wav
US-EN-Morgan-Freeman-Starting-Party-Mode.wav
US-EN-Morgan-Freeman-Starting-Romance-Mode.wav
US-EN-Morgan-Freeman-Turning-Off-All-The-Lights.wav
US-EN-Morgan-Freeman-Turning-Off-The-Air-Conditioner.wav
US-EN-Morgan-Freeman-Turning-Off-The-Bar-Lights.wav
US-EN-Morgan-Freeman-Turning-Off-The-Chandelier.wav
US-EN-Morgan-Freeman-Turning-Off-The-Family-Room-Lights.wav
US-EN-Morgan-Freeman-Turning-Off-The-Hallway-Lights.wav
US-EN-Morgan-Freeman-Turning-Off-The-Kitchen-Light.wav
US-EN-Morgan-Freeman-Turning-Off-The-Light.wav
US-EN-Morgan-Freeman-Turning-Off-The-Lights.wav
US-EN-Morgan-Freeman-Turning-Off-The-Mood-Lights.wav
US-EN-Morgan-Freeman-Turning-Off-The-TV.wav
US-EN-Morgan-Freeman-Turning-On-The-Air-Conditioner.wav
US-EN-Morgan-Freeman-Turning-On-The-Bar-Lights.wav
US-EN-Morgan-Freeman-Turning-On-The-Chandelier.wav
US-EN-Morgan-Freeman-Turning-On-The-Family-Room-Lights.wav
US-EN-Morgan-Freeman-Turning-On-The-Hallway-Lights.wav
US-EN-Morgan-Freeman-Turning-On-The-Kitchen-Light.wav
US-EN-Morgan-Freeman-Turning-On-The-Light.wav
US-EN-Morgan-Freeman-Turning-On-The-Lights.wav
US-EN-Morgan-Freeman-Turning-On-The-Mood-Lights.wav
US-EN-Morgan-Freeman-Turning-On-The-TV.wav
US-EN-Morgan-Freeman-Vacate-The-Premises.wav
US-EN-Morgan-Freeman-Water-Detected-In-Basement.wav
US-EN-Morgan-Freeman-Water-Detected-In-Garage.wav
US-EN-Morgan-Freeman-Water-Detected-In-Kitchen.wav
US-EN-Morgan-Freeman-Welcome-Home.wav
US-EN-Morgan-Freeman-Wife-Is-Arriving.wav
```
