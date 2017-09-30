---
layout: page
title: "Xiaomi Mi Robot Vacuum"
description: "Instructions how to integrate your Xiaomi Mi Robot Vacuum within Home Assistant."
date: 2017-05-05 18:11
sidebar: true
comments: false
sharing: true
footer: true
logo: xiaomi.png
ha_category: Vacuum
ha_release: 0.51
ha_iot_class: "Local Polling"
---

小米扫地机器人平台 `xiaomi_vacuum` 帮助你在 HA 中控制扫地机器人。

目前支持的控制指令有启动 `turn_on`， 暂停 `pause`，原地停止 `stop`，回到充电桩 `return_to_home`，关闭并回到充电桩 `turn_off`，定位 `locate`，定点打扫 `clean_spot`，设定吸力 `set_fanspeed` 以及远程控制。

Please follow the instructions on [Retrieving the Access Token](/components/xiaomi/#retrieving-the-access-token) to get the API token to use in the `configuration.yaml` file.

请使用手机和米家 app 根据下述教程从 SQLite 文档中获取机器人的秘钥 token，从而将扫地机接入 HA 平台。

进行配置前，请确保在对应平台上已经安装`python-mirobo` 的依赖库 `libffi-dev`：

```bash
apt-get install libffi-dev
```

<p class='note warning'>
如果你的 HA 是在虚拟环境 [Virtualenv](/docs/installation/virtualenv/#upgrading-home-assistant)下运行的，在运行下列命令前请确保已切换到虚拟环境。</p>

```bash
$ sudo su -s /bin/bash homeassistant
$ source /srv/homeassistant/bin/activate
```

请根据手机平台选择获取 token 的方法：

### Windows & Android
1. 通过米家 app 连接扫地机器人；
2. 打开手机的开发者模式和 USB 调试模式，将手机连至电脑；
3. 获取 ADB 工具：https://developer.android.com/studio/releases/platform-tools.html
4. 创建 com.xiaomi.smarthome 的应用备份:

```bash
.\adb backup -noapk com.xiaomi.smarthome -f backup.ab
```
5. 如果你的终端显示如下信息: "More than one device or emulator"，使用下列指令显示所有设备：
```bash
.\adb devices
```
并执行下列指令：

```bash
.\adb -s DEVICEID backup -noapk com.xiaomi.smarthome -f backup.ab # (with DEVICEID the device id from the previous command)
```
6. 在手机上选择确认备份，请勿输入任何密码；
7. 获取 ADB 备份提取工具：https://sourceforge.net/projects/adbextractor/
8. 提取所有备份文件：

```bash
java.exe -jar ../android-backup-extractor/abe.jar unpack backup.ab backup.tar ""
```
9. 解压 ".tar" 文件；
10. 使用 SQLite Manager 等类似工具，打开 sqlite 文件 miio2.db；
11. 获取 "devicerecord" 数据表，即 token。


### macOS & iOS
1. 通过米家 app 连接扫地机器人；
2. 通过 iTunes 创建备份；
3. 安装 iBackup Viewer ：http://www.imactools.com/iphonebackupviewer/
4. 提取文件 **`/raw data/com.xiami.mihome/1234567_mihome.sqlite`** 至你的电脑， _1234567_ 可以是任何数字，请自行寻找
5. 使用 notepad 或其他编译器打开文件。你会看到一系列的设备和秘钥，你需要的 token 在 **`ZToken`** 栏中，类似**`123a1234567b12345c1d123456789e12`**.

## {% linkable_title Configuration %}
获取 token 后，在 `configuration.yaml`中添加如下配置：

```yaml
vacuum:
  - platform: xiaomi
    host: 192.168.1.2
    token: YOUR_TOKEN
```

变量说明：
- **host** (*必需*): 机器人 IP
- **token** (*必需*): 机器人 token
- **name** (*可选*): 机器人昵称


### {% linkable_title Platform services %}

除了 HA 中所有扫地机器人所能使用的通用指令 [vacuum component services](/components/vacuum#component-services) (`turn_on`, `turn_off`, `start_pause`, `stop`, `return_to_home`, `locate`, `set_fanspeed` and `send_command`)，小米平台另外还支持一些特殊的远程操控指令，包括：
`xiaomi_remote_control_start`, `xiaomi_remote_control_stop`, `xiaomi_remote_control_move` 和 `xiaomi_remote_control_move_step`。

#### {% linkable_title Service `vacuum/xiaomi_remote_control_start` %}

使用特殊指令，首先请开启扫地机器人的手动控制模式。开启后你便可以使用`remote_control_move`和 `remote_control_stop`手动控制指令了。

| 属性    | 可选性 | 描述                                           |
|---------------------------|----------|-------------------------------------------------------|
| `entity_id`               |      是 | 指明 ID 仅对部分设备有效，否则全局响应        |

#### {% linkable_title Service `vacuum/xiaomi_remote_control_stop` %}

退出远程控制模式

| 属性    | 可选性 | 描述                                           |
|---------------------------|----------|-------------------------------------------------------|
| `entity_id`               |      是 | 指明 ID 仅对部分设备有效，否则全局响应        |

#### {% linkable_title Service `vacuum/xiaomi_remote_control_move` %}

远程控制扫地机器人，确保操作前已经开启远程控制模式 `remote_control_start`。

| 属性    | 可选性 | 描述                                           |
|---------------------------|----------|-------------------------------------------------------|
| `entity_id`               |      是 | 指明 ID 仅对部分设备有效，否则全局响应        |
| `velocity`                |       否 | 速度，值区间为 -0.29 至 0.29                        |
| `rotation`                |       否 | 旋转， 值区间为 -179° 至 179°       |
| `duration`                |       否 | 持续时间     |


#### {% linkable_title Service `vacuum/xiaomi_remote_control_move_step` %}

使用此指令进入遥控模式，执行一项控制命令，然后退出手动控制模式。

| 属性    | 可选性 | 描述                                           |
|---------------------------|----------|-------------------------------------------------------|
| `entity_id`               |      是 | 指明 ID 仅对部分设备有效，否则全局响应        |
| `velocity`                |       否 | 速度，值区间为 -0.29 至 0.29                        |
| `rotation`                |       否 | 旋转， 值区间为 -179° 至 179°        |
| `duration`                |       否 | 持续时间     |


