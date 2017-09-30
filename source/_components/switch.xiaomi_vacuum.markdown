---
layout: page
title: "Xiaomi Mi Robot Vacuum"
description: "Instructions how to integrate your Xiaomi Mi Robot Vacuum within Home Assistant."
date: 2017-05-05 18:11
sidebar: true
comments: false
sharing: true
footer: true
logo: xiaomi_vacuum.png
ha_category: Switch
ha_release: 0.48
---

[小米扫地机器人平台](http://www.mi.com/roomrobot/) `xiaomi_vacuum` 帮助你在 HA 中控制扫地机器人。 

目前支持的控制指令有启动 `start` 和停止 `stop` （回到充电桩）。


{% linkable_title Getting started %}

请使用手机和米家 app 根据下述教程从 SQLite 文档中获取机器人的秘钥 `token`，从而将扫地机接入 HA 平台。

<p class='note warning'>
如果你的 HA 是在虚拟环境 [Virtualenv](/docs/installation/virtualenv/#upgrading-home-assistant) 下运行的，在运行下列命令前请确保已切换到虚拟环境。</p>

```bash 
$ sudo su -s /bin/bash homeassistant
$ source /srv/homeassistant/bin/activate
```

请根据手机平台选择获取 token 的方法：

### Windows 和 Android
1. 通过米家 app 连接扫地机器人；
2. 打开手机的开发者模式和 USB 调试模式，将手机连至电脑；
3. 获取 ADB 工具： https://developer.android.com/studio/releases/platform-tools.html
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
9.解压 ".tar" 文件；
10. 使用 SQLite Manager 等类似工具，打开 sqlite 文件 `miio2.db`；
11. 获取 "devicerecord" 数据表，即 token。


### macOS 和 iOS
1. 通过米家 app 连接扫地机器人；
2. 通过 iTunes 创建备份；
3. 安装 iBackup Viewer ： http://www.imactools.com/iphonebackupviewer/
4. 提取文件 `/raw data/com.xiami.mihome/_mihome.sqlite` 至你的电脑
5. 使用 notepad 或其他编译器打开文件。你会看到一系列的设备和秘钥，找到小米机器人。 

{% linkable_title Configuration %}
获取 token 后，在 `configuration.yaml`中添加如下配置：
```yaml
- platform: xiaomi_vacuum
  name: 'name of the robot'
  host: 192.168.1.2
  token: your-token-here
```

变量说明：
- **name** (*可选*): 机器人昵称
- **host** (*必需*): 机器人 IP
- **token** (*必需*):  机器人 token 

