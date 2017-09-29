---
layout: page
title: "Input Text"
description: "Instructions how to integrate the Input Text component into Home Assistant."
date: 2016-03-15 06:00
sidebar: true
comments: false
sharing: true
footer: true
logo: home-assistant.png
ha_category: Automation
ha_release: 0.53
---

`input_text` 组件让用户可以通过前端输入可控设备的具体数值或者定义自动化的条件。输入框中值的改变将会触发设备的『状态改变事件』。此『事件』`automation（自动化）` 的触发条件。

```yaml
# 例如
input_text:
  text1: #[alias]
    name: Text 1
    initial: Some Text
  text2:
    name: Text 2
    min: 8
    max: 40
  text3:
    name: Text 3
    pattern: '[a-fA-F0-9]*'
```

变量说明：

- **[alias]** (*必须*): 别名，必须为英文（译者注）。
- **min** (*可选*): 最短字数。默认为 `0`。
- **max** (*可选*): 最长字数。 默认为 `100`。
- **name** (*可选*): 昵称。
- **initial** (*可选*): 初始值，默认为空。
- **pattern** (*可选*): 数值的正则表达式，默认为空，在系统中被识别为  `.*`.


