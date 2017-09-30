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

小米 WIFI 设备平台 `xiaomi_miio` 能让你在 HA 中控制扫地机器人。

目前支持的控制指令有启动 `turn_on`， 暂停 `pause`，原地停止 `stop`，回到充电桩 `return_to_home`，关闭并回到充电桩 `turn_off`，定位 `locate`，定点打扫 `clean_spot`，设定吸力 `set_fanspeed` 以及远程控制。

请先按照 [此教程](/components/vacuum.xiaomi/#retrieving-the-access-token) 获取设备 `token`，之后在 `configuration.yaml` 文件中添加以下配置：

```yaml
vacuum:
  - platform: xiaomi_miio
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

Start the remote control mode of the vacuum cleaner. You can then move it with `remote_control_move`, when done call `remote_control_stop`.

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


### {% linkable_title Attributes %}

除了 HA 上所有扫地机器人共有的默认属性 [`vacuum` component attributes] (`battery_icon`, `cleaned_area`, `fan_speed`, `fan_speed_list`, `status`, `params`)，小米扫地机器人另外拥有一些特别的属性。

它们是：最近清洁时间 `cleaning_time`、勿扰模式 `do_not_disturb`，主刷剩余寿命 `main_brush_left`，边刷剩余寿命 `side_brush_left`，滤网剩余寿命 `filter_left`，清洁通道统计`cleaning_count`，清洁区域统计 `total_cleaned_area` 以及清洁时间统计 `total_cleaning_time`，详见下表：

| 属性                 | 单位 | 说明                                           |
|---------------------------|---------------------|-------------------------------------------------------|
| `do_not_disturb`          |                     | 勿扰模式开启关闭状态                                     |
| `cleaning_time`           | minutes 分钟             | 最近清洁时间                | 
| `cleaned_area`            | square meter 平方米        | 最近清洁区域统计            |
| `main_brush_left`         | hours 小时             | 主刷剩余寿命 |
| `side_brush_left`         | hours 小时              | 边刷剩余寿命 |
| `filter_left`             | hours 小时              | 滤网剩余寿命     |
| `cleaning_count`          |                     | 总清洁通道数                      |
| `total_cleaned_area`      | square meter 平方米       | 总清洁范围                    |
| `total_cleaning_time`     | minutes 分钟             | 总清洁时间                        |

### {% linkable_title Retrieving the Access Token %}

<p class='note'>
小米设备必须获取 `token` 后才可以接入 HA，网关及子设备需要获取 16 位的 `token`，获取过程较容易，直接通过『米家 app』即可。而小米 WIFI 设备需要获取 32 位的 `token`，过程比较麻烦，详见下方说明：
</p>

请使用手机和米家 app 根据下述教程从 SQLite 文档中获取机器人的秘钥 token，从而将扫地机接入 HA 平台。

进行配置前，请确保在对应平台上已经安装 `python-mirobo` 对应的依赖库：

```bash
$ sudo apt-get install libffi-dev libssl-dev
```

如果你的 HA 是在虚拟环境 [Virtualenv](/docs/installation/virtualenv/#upgrading-home-assistant) 下运行的，在运行下列命令前请确保已切换到虚拟环境：

```bash
$ sudo su -s /bin/bash homeassistant
$ source /srv/homeassistant/bin/activate
```

请根据平台选择获取 token 的方法：

#### {% linkable_title Windows and Android %}

1. 通过米家 app 连接扫地机器人；
2. 打开手机的开发者模式和 USB 调试模式，将手机连至电脑；
3. 获取 ADB 工具：https://developer.android.com/studio/releases/platform-tools.html
4. 创建 com.xiaomi.smarthome 的应用备份:

```bash
.\adb backup -noapk com.xiaomi.smarthome -f backup.ab
```
5. 如果你的终端显示如下信息: "有多个设备或模拟器"，使用下列指令显示所有设备：
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


#### {% linkable_title Linux and Android (rooted!) %}
此教程建立在 安卓手机已 root 的情况下。

1. 通过米家 app 连接设备；
2. 打开手机的开发者模式和 USB 调试模式，将手机连至电脑；
3. 获取 ADB 工具，在终端中输入 `apt-get install android-tools-adb`
4. `adb devices` 将会显示你的设备
5. `adb root` (仅适用于 Linux development builds： `ro.debuggable=1`)
6. `adb shell`
7. `echo "select name,localIP,token from devicerecord;" | sqlite3 /data/data/com.xiaomi.smarthome/databases/miio2.db` 返回设备 token


