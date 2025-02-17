---
translatedFrom: en
translatedWarning: 如果您想编辑此文档，请删除“translatedFrom”字段，否则此文档将再次自动翻译
editLink: https://github.com/ioBroker/ioBroker.docs/edit/master/docs/zh-cn/adapterref/iobroker.wireless-mbus/README.md
title: ioBroker.wireless-mbus
hash: Zps/t1ZPAmNhPPNGkXQ8XWPlEFY9UkblIupZ55kFsEY=
---
![标识](../../../en/adapterref/iobroker.wireless-mbus/admin/wireless-mbus.png)

# IoBroker.wireless-mbus
该适配器允许从支持的接收器接收无线 M-Bus 数据。设备实现的范围各不相同，但可以为所有列出的设备配置 wMBus 模式。

* 嵌入 WMB 模块
* Amber Wireless AMB8465（**注意：** 命令模式（UART_CMD_Out_Enable）已启用！）
* IMST iM871A
* CUL

WMBUS 堆栈已从 FHEM 项目“重新移植”，并进行了广泛的修复和重构。测试是使用从互联网上获取的原始数据、OMS 样本数据和来自 jmbus 库的一些测试数据完成的。一些边缘情况仍然未经测试。

设备创建、更新等主要基于 Apollon77 的 M-Bus 适配器（见下文）。

如果适配器收到加密电报，AES 密钥配置选项卡应自动列出设备 ID。

如果解析器失败，原始电报数据将保存到 info.rawdata 状态。

*注意：* 在 C 模式下，Amber 接收器似乎在一段时间（或接收到的消息量）后崩溃？硬件缺陷？

## 链接：
* [WMBus 堆栈模块](https://github.com/mhop/fhem-mirror/blob/master/fhem/FHEM/WMBus.pm)
* [ioBroker.mbus](https://github.com/Apollon77/ioBroker.mbus)
* [原始 WMBUS 堆栈：wm-bus](https://github.com/soef/wm-bus)
* [M-Bus 协议](http://www.m-bus.com/files/MBDOC48.PDF)
* [OMS 规范](https://oms-group.org/en/download4all/oms-specification/)

＃＃ 初始设置
初始设置需要配置基础知识（与 wmbus 的硬件连接）并为要收集的所有加密 wmbus 节点设置 AES 密钥。最棘手的部分是 AES 密钥。

### 基本设置
这需要选择合适的 USB 设备和正确的波特率（**通常** IMST：57600 波特；琥珀色：9600 波特；Embit：9600 波特）。大多数仪表将以“T 模式”发送。

### 其他选项
* **更新未改变的状态**：当电报到达时，所有状态都会被更新，即使它们的值没有改变。 （默认：开启）
* **支持紧凑帧缓存**：为了支持紧凑电报（由某些（卡姆鲁普？）设备使用），所有接收到的电报的结构都被缓存。这意味着通常每个设备只有一个缓存条目。如果您没有任何发送紧凑电报的设备，您可以禁用它以节省一些性能和内存。 （默认：关闭）
* **将能量单位强制转换为 kWh**：所有能量单位（Wh 和 J）都将转换为 kWh。 （默认：关闭）
* **连续失败后临时阻止设备**：如果同一设备的 10 个连续报文未成功解析，则设备将被忽略，直到适配器重新启动（默认值：开启）

### AES 密钥
设备标识符是制造商代码和设备 ID 的组合（例如 AAA-12345678）。密钥可以作为 16 个字符的纯文本密钥输入，也可以作为 32 个字符（16 个字节）的十六进制字符串输入。

设置密钥的最简单方法是在没有任何密钥设置的情况下启动适配器并等待加密的电报，之后适配器会生成一个带有“UNKNOWN”密钥的条目。然后就可以填写对应的key并保存设置了。如果您看到您不知道或只想摆脱的设备（例如来自邻居的设备），您可以在阻止的设备选项卡中输入它们。

＃＃ 去做
* 为 S 模式接收器发送电报？
* CUL 支持需要测试

## Changelog

### 0.7.5
* (ChL) Fix timeout handling - if no problems occur this will be republished as 1.0.0

### 0.7.3 / 0.7.4
* (ChL) Try to improve CUL support

### 0.7.1 / 0.7.2
* (ChL) Rename to ioBroker.wireless-mbus to be able to publish to npm
* (ChL) Fix block list, admin page logo and repo url in package.json

### 0.7.0
* (ChL) Change main adapter code to class
* (ChL) Include actual (machine) translations besides English and German
* (ChL) Upgrade denpendencies
* (ChL) Add test for wmbus decoder
* (ChL) Add integration tests
* (ChL) Add github workflow

### 0.6.2
* (ChL) Improve admin page to handle custom serialport path
* (ChL) Add option to turn automatic blocking of devices off
* (ChL) Add "Simple Hexstring" receiver for testing purposes
* (ChL) Internal refactoring

### 0.6.0 / 0.6.1
* (ChL) Upgrade of serialport library to 9.2.0
* (ChL) experimental CUL support

### 0.5.2
* (ChL) fix for connection indicator with js-controller 2.x

### 0.5.1
* (ChL) Small fixes
* (ChL) Internal telegram parser now supports wired M-Bus frames (not used - for testing / developing purpose)
* (D Glaser) Added timestamp of last update to device info
* (D Glaser/ChL) Added some setup documentation to README

### 0.5.0
* (ChL) Basic support for Techem devices
* (ChL) Option to force energy units (Wh and J) to kWh - BEWARE this is not really backwards compatible. Old states will keep their "old" unit, but display the adjusted value!

### 0.4.7
* (ChL) Block devices after 10 consecutive failed parse attempts until adapter restart
* (ChL) Assign roles derived from units (as does the mbus adapter)

### 0.4.6
* (ChL) Support for (Kamstrup?) compact frames through data record cache (pre-defined frames have been removed!)

### 0.4.5
* (ChL) Append device ids with key "UNKNOWN" at startup to needskey

### 0.4.2 / 0.4.3 / 0.4.4
* (ChL) Small fixes

### 0.4.1
* (ChL) basic IMST iM871A support

### 0.4.0
* (ChL) better Amber Stick support
* (ChL) Compact mode?
* (ChL) Nicer state names
* (ChL) wMBus mode partially selectable

### 0.3.0
* (ChL) Implemented all VIF types from MBus doc
* (ChL) VIF extensions are handled better (again)
* (ChL) reorganised VIF info
* (ChL) reorganised receiver handling
* (ChL) blocking of devices possible

### 0.2.0 (not tagged)
* (ChL) Dramatically improved parser: support for security mode 7, frame type B, many small fixes
* (ChL) VIF extensions are handled better, but correct handling is still not fully clear
* (ChL) CRCs are checked and removed if still present
* (ChL) raw data is saved if parser fails

### 0.1.0
* (ChL) initial release

## License

Copyright (c) 2019 ISFH - Institute for Solar Energy Research www.isfh.de
Copyright (c) 2021 Christian Landvogt

Licensed under GPLv2. See [LICENSE](LICENSE) and [NOTICE](NOTICE)