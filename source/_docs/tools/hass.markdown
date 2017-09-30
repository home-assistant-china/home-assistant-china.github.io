---
layout: page
title: "hass"
description: "Description of hass."
release_date: 2017-02-23 11:00:00
sidebar: true
comments: false
sharing: true
footer: true
---

Home Assistant 所使用的命令是 `hass`.


```bash
$ hass -h
usage: hass [-h] [--version] [-c path_to_config_dir] [--demo-mode] [--debug]
            [--open-ui] [--skip-pip] [-v] [--pid-file path_to_pid_file]
            [--log-rotate-days LOG_ROTATE_DAYS] [--runner] [--script ...]
            [--daemon]

Home Assistant: Observe, Control, Automate.

optional arguments:
  -h, --help            显示此帮助信息并退出
  --version             显示 HA 版本号并退出
  -c path_to_config_dir, --config path_to_config_dir
                        指明 HA 配置文件所在文件夹
  --demo-mode           演示模式下启动 HA
  --debug               Debug 模式下启动 HA
  --open-ui             开启 UI
  --skip-pip            启动时跳过安装必需的pip包
  -v, --verbose         开启详细日志记录
  --pid-file path_to_pid_file
                        后台运行文件PID file 所在文件夹
  --log-rotate-days LOG_ROTATE_DAYS
                        允许日志回滚和特定日期跳转

                        Enables daily log rotation and keeps up to the
                        specified days
  --log-file LOG_FILE   Log file to write to. If not set, CONFIG/home-
                        assistant.log is used

  --runner              On restart exit with code 100
  --script ...          执行命令
  --daemon              后台运行 HA
```


