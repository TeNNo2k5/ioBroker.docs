---
translatedFrom: en
translatedWarning: 如果您想编辑此文档，请删除“translatedFrom”字段，否则此文档将再次自动翻译
editLink: https://github.com/ioBroker/ioBroker.docs/edit/master/docs/zh-cn/adapterref/iobroker.openknx/README.md
title: ioBroker.openknx
hash: 9RoTOkmpsdKGiO0Z8+uYJrrII46BeOks0DE+0bg/RQw=
---
![标识](../../../en/adapterref/iobroker.openknx/admin/openknx.png)

![NPM 版本](http://img.shields.io/npm/v/iobroker.openknx.svg)
![下载](https://img.shields.io/npm/dm/iobroker.openknx.svg)
![安装数量](https://iobroker.live/badges/openknx-installed.svg)
![稳定存储库中的当前版本](https://iobroker.live/badges/openknx-stable.svg)
![新PM](https://nodei.co/npm/iobroker.openknx.png?downloads=true)

# IoBroker.openknx
**测试：** ![测试和发布](https://github.com/iobroker-community-adapters/ioBroker.openknx/workflows/Test%20and%20Release/badge.svg)

该适配器用作 Iobroker 和您的 KNX IP 网关之间的通信接口。
适配器允许通过导入 ETS 组地址 xml 导出来自动生成 iobroker 通信对象。
所有生成的通信对象最初都配置为可读和可写，在适配器重新启动时从 knx 总线获取值。

＃ 安装
该适配器在最新 / beta 存储库中可用。如果在 ioBroker 系统设置中将其选为标准存储库，则可以在“openknx”下的适配器列表中搜索该适配器并通过单击 + 符号进行安装。另一种方法是通过 Github 符号以专家模式安装，方法是选择“来自 Github”并搜索 openknx。

# 适配器配置
![设置](../../../en/adapterref/iobroker.openknx/docs/pictures/setting.png) 按“保存并关闭”或“保存”重新启动适配器并接管更改。
启动时，适配器尝试读取所有具有自动读取标志（默认设置）的 GroupAdress。
这可能需要一段时间，并且会在您的 KNX 总线上产生更高的负载。这可确保适配器从一开始就使用最新值运行。
自动读取是在适配器启动或重新启动后与 knx 总线的第一次连接时完成的，而不是在每次 knx 重新连接时完成的。
适配器安装后，打开适配器配置。填写：

### KNX 网关 IP
KNX IP 网关的 IP。

＃＃＃ 港口
这通常是 KNX IP 网关的端口 3671。

###物理。 KNX 地址
以 1/1/1 格式填写 KNX IP 网关的物理地址。
如果需要，可以手动转换两级组地址。

### 本地 IPv4 网络接口
连接到 KNX IP 网关的接口。

＃＃＃ 探测
通过标准化协议搜索给定网络接口上所有可用的 KNX IP 网关。

### 帧延迟 [ms]
此设置通过将数据帧限制为特定速率来保护 KNX 总线免受数据泛滥。未发送的帧被放入 fifo 缓冲区。如果您在日志中遇到与 KNX IP 网关断开连接的情况，请增加此数字。

### 仅添加新对象
如果选中，导入将跳过覆盖现有通信对象。

### 从 ETS 导入 XML
![ETS 出口](../../../en/adapterref/iobroker.openknx/docs/pictures/exportGA.png)

1. 在 ETS 中的 Group Addresses 中，选择 export group address 并选择 XML export in latest format version。

不支持 ETS4 格式，它不包含 DPT 信息。

2. 通过 GA XML-Import 对话框将您的 ETS Export XML 上传到适配器中
3.文件选择后立即开始导入，完成后给出状态报告。

成功导入后，一条消息会显示识别出的对象数量。
错误对话框将在导入过程中发现问题，并提示如何清理 ets 数据库。
可以在日志中找到其他信息。

ETS 配置提示：如果 GA 和使用此 GA 的通信对象中有不同的 DPT 子类型，则 ETS 似乎使用编号最小的 DPT 类型。
在这种情况下，手动确保所有元素都使用相同的所需数据类型。
没有 DPT 基本类型的 GA 无法使用此适配器导入。 ETS4 项目必须转换为 ETS5 或更高版本，并且 DPT 必须设置为 GA。

### 别名
KNX 设备可以具有属于命令 ga 的状态反馈的 ga。某些应用程序（例如某些 VIS 小部件）需要组合状态和驱动对象。您可以将这些状态组合成一个别名，方法是使用一个单独的别名 id 写入并使用另一个别名 id 读取。该菜单有助于根据命名约定和给定的过滤规则创建匹配对。
在此处查找更多信息 https://www.iobroker.net/#en/documentation/dev/aliases.md

### 正则表达式
过滤规则。

### 最小相似度
定义匹配算法过滤相似条目的严格程度。

### 别名路径
生成别名的对象文件夹。

### 在搜索中包含组范围
包括路径在内的整个名称用于检查相似性。

# 适配器迁移提示
## 迁移节点红色
- 在右侧菜单中，选择导出
- 选择所有流程，下载
- 在文本编辑器中替换 knx.0。使用 openknx.0。
- 右侧菜单，选择导入
- 选择更改的文件
- 在对话框中选择 Flows (Subflows, Configuration-Nodes only if they are affected) -> new tabs get added
- 手动删除旧流程

## 迁移 VIS
- 打开可见编辑器
- 设置 -> Projekt-Export/import -> Exportieren normal
- 在编辑器中打开 Zip 文件和 vis-views.json
- 搜索替换 knx.0。使用 openknx.0。
- 将 vis-views.json 和 vis-user.css 压缩到一个 zip 文件中
- 设置 -> Projekt 导出/导入 -> 导入
- 在拖放区移动 zip 文件
- 项目名称 = 主要
- 导入项目

## 迁移脚本
- 打开脚本
- 3 个点 -> 导出所有脚本
- 打开 Zip 文件并在编辑器中打开文件夹
- 搜索用 openknx.0 替换 knx.0
- 将所有更改的文件压缩到一个 zip 文件中
- 3 个点 -> 导入脚本
- 在拖放区移动 zip 文件

## 迁移 Grafana
- 浏览所有仪表板并选择共享 - 导出 - 保存到文件
- 在文本编辑器中替换 knx.0。使用 openknx.0。
- 要导入仪表板，请单击侧面菜单中的 + 图标，然后单击导入。
- 从这里您可以上传仪表板 JSON 文件
- 选择导入（覆盖）

## 迁移涌入
- 使用命令 influx 登录到您的 IOBroker 服务器
- 使用 iobroker（或您通过命令 show databases 列出的特定数据库）
- 列出条目：显示测量值
- 使用命令复制表：select * into "entry_new" from "entry_old"；
- 为新对象 entry_new 设置流入启用

# 如何使用适配器和基本概念
### ACK 标志
应用程序不应设置 ack 标志，如果数据更新，则通过 ack 标志从该适配器通知应用程序。
KNX 堆栈在收到组地址时设置链接的 ioBroker 对象的确认标志。
KNX 上发送的帧不会导致写入对象的确认。

### Node Red 复杂数据类型示例
创建连接到 ioBroker out 节点的函数节点，该节点与 DPT2 的 KNX 对象连接。
msg.payload = {“优先级”：1，“数据”：0}；返回味精；

# 日志级别
启用专家模式以启用不同日志级别之间的切换。默认日志级别是信息。
![日志级别](../../../en/adapterref/iobroker.openknx/docs/pictures/loglevel.png)

# IOBroker 通信对象描述
ioBroker 定义对象来保存通信接口设置。
GA 导入生成遵循 ga 主组/中间组方案的通信对象文件夹结构。每个组地址都是一个对象，具有以下自动生成的数据。

ioBroker 状态角色 (https://github.com/ioBroker/ioBroker/blob/master/doc/STATE_ROLES.md) 默认具有值“状态”。一些更细化的值是从 DPT 派生的，例如 Date 或 Switch。

自动读取设置为假，从 DPT 可以清楚地看出这是一个触发信号。这适用于场景编号。

{ "_id": "path.and.name.to.object", //源自 KNX 结构 "type": "state", "common": { //这里的值可以被 iobroker 解释 "desc": "Basetype: 1-bit value, Subtype: switch", //informative, from dpt "name": "Aussen Melder Licht schalten", //来自 ets export "read": true, //default set, if false传入的总线值不更新对象 "role": state, //默认状态, 派生自 DPT "type": "boolean", //boolean, number, string, object, 派生自 dpt "unit": "", //派生自 dpt "write": true //默认为 true，如果对象上的设置更改触发 knx 写入，则成功。 write sets then ack flag to true }, "native": { //这里的值可以被openknx适配器解释 "address": "0/1/2", //knx组地址 "answer_groupValueResponse": false, //default false，如果设置为 true，则适配器响应 GroupValue_Read 上的值 "autoread": true, //默认为非触发信号的 true，适配器在开始时发送 GroupValue_read 以同步其状态 "bitlength": 1, //size ob knx 数据，源自 dpt "dpt": "DPT1.001", //DPT "encoding": { //信息性 "0": "Off", "1": "On" }, "force_encoding": "", // informationative "signness": "", //informative "valuetype": "basic" //复合意味着通过特定的 javascript 对象设置 }, "from": "system.adap ter.openknx.0”，“用户”：“system.user.admin”，“ts”：1638913951639 }

#适配器通信接口说明
Handeled DPTs 是： 1-21,232,237,238 Unhandeled DPTs 被写为原始缓冲区，接口是十六进制数字的顺序字符串。例如，写入 '0102feff' 以在总线上发送值 0x01 0x02 0xfe 0xff。
在使用数字数据类型的地方，请注意接口值可以缩放。

### API 调用
ioBroker 将状态定义为通信接口。

setState( @param {string} 具有路径的对象的 ID @param {object|string|number|boolean} 状态简单值或具有属性的对象。
{ val: value, ack: true|false, optional, 按照惯例应该是 false忽略自：origin，可选，默认 - 此适配器 c：注释，可选，将其设置为值 GroupValue_Read 以触发对该对象的总线读取，给定 StateValue 被忽略 expire：expireInSeconds 可选，默认 - 0 lc：timestampMS 可选，默认 -计算值 } @param {boolean} [ack] 可选，按照约定应该为 false @param {object} [options] 可选，用户上下文 @param {ioBroker.SetStateCallback} [callback] 可选，返回错误和 id

触发 GroupValue_Read 的示例：

setState(myState, {val: false, ack: false, c:'GroupValue_Read'}); setState(myState, {val: false, ack: false, q:0x10});

GroupValue_Read 注释不适用于 javascript 适配器。请改用 qualityAsNumber 值 0x10。

### 所有 DPT 的描述
| KNX DPT | javascript 数据类型 |特殊价值 |取值范围 |备注 |
| --------- | ---------------------- | ---------------------------------------------------------------------------------------------------- | ----------------------------------------- | --------------------------------------------------- |
| DPT-1 |布尔值 | |假的，真的 ||
| DPT-2 |对象 | {“优先级”：1 位，“数据”：1 位} | - ||
| DPT-3 |对象 | {“decr_incr”：1 位，“数据”：2 位} | - ||
| DPT-18 |对象 | {"save_recall":0,"scenenumber":0} | - |从自动读取中删除数据点类型 DPT_SceneControl|
| DPT-21 |对象 | {"outofservice":0,"fault":0,"overridden":0,"inalarm":0,"alarmunack":0} | - ||
| DPT-232 |对象 | {红色：0..255，绿色：0.255，蓝色：0.255} | - ||
| DPT-237 |对象 | {"address":0,"addresstype":0,"readresponse":0,"lampfailure":0,"ballastfailure":0,"convertorerror":0} | - ||
| DPT-4 |字符串 | |一个字符作为 8 位字符发送 ||
| DPT-16 |字符串 | |一个字符作为 16 个字符的字符串发送 ||
| DPT-5 |号码 | | 8 位无符号值 ||
| DPT-5.001 |号码 | | 0..100 [%] 缩放为 1 字节 ||
| DPT-5.003 |号码 | | 0..360 [°] 缩放为 1 字节 ||
| DPT-6 |号码 | | 8 位有符号 -128..127 ||
| DPT-7 |号码 | | 16 位无符号值 ||
| DPT-8 |号码 | | 2 字节有符号值 -32768..32767 ||
| DPT-9 |号码 | | 2 字节浮点值 ||
| DPT-14 |号码 | | 4 字节浮点值 ||
| DPT-12 |号码 | | 4 字节无符号值 ||
| DPT-13 |号码 | | 4 字节有符号值 ||
| DPT-15 |号码 | | 4 字节 ||
| DPT-17 |号码 | | 1 字节 | DPT_SceneNumber 从自动读取中删除|
| DPT-20 |号码 | | 1 字节 ||
| DPT-238 |号码 | | 1 字节 ||
| DPT-10 |日期对象的编号 | | - ||
| DPT-11 |日期对象的编号 | | - ||
| DPT-19 |日期对象的编号 | | - ||
| DPT-26 |字符串 |例如00010203.. | - |数据点类型 DPT_SceneInfo 未由 autread| 读取 |
| DPT-238 |字符串 |例如00010203.. | - | autread 未读取 DPT_SceneConfig|
|休息 |字符串 |例如00010203.. | - ||

只有时间和日期信息与基于 KNX 时间的数据类型交换，例如DPT-19 具有不受支持的信号质量字段。

对象发送和接收值的类型为 boolean DPT1)、数字（缩放或未缩放）、字符串。
DPT 2 '期望对象 {"priority":0,"data":1}' 接收提供相同类型的字符串化对象。
其他联合 DPT 具有类似的对象符号。
DPT19 需要来自日期对象的数字，Iobroker 无法处理对象，无法从时间戳派生的 KNX ko 字段未实现，例如。质量标志。

日期和时间 DPT (DPT10, DPT11) 请记住，Javascript 和 KNX 具有非常不同的时间和日期基本类型。
DPT10 是时间 (hh:mm:ss) 加上“星期几”。这个概念在 JS 中不可用，因此您将获取/设置常规的 Date Js 对象，但请记住您需要忽略日期、月份和年份。转换为“Mon, Jul 1st 12:34:56”的完全相同的数据报将在一周后评估为“Mon, Jul 8th 12:34:56”的完全不同的 JS 日期。被警告！ DPT11 是日期 (dd/mm/yyyy)：同样适用于 DPT11，您需要忽略时间部分。

（DPT 的 KNX 规范 https://www.knx.org/wAssets/docs/downloads/Certification/Interworking-Datapoint-types/03_07_02-Datapoint-Types-v02.02.01-AS.pdf）

### 组值写入
通过写入通信对象触发发送。
当总线上接收到写帧时触发通信对象。

### 组值读取
可以通过编写带有注释的通信对象来触发发送。
接收，如果配置将触发实际c.o的组值响应（限制：此时组值写入）。值，见下文。

### 组值响应
如果 answer_groupValueResponse 设置为 true，则适配器将以 GroupValue_Response 回复先前收到的 GroupValue_Read 请求。
这是 KNX 读取标志。只有总线上的一个 KO 或 IOBroker 对象应该设置此标志，理想情况下是最了解状态的那个。

### 映射到 KNX 标志
KNX 对象标志定义了它们所代表的对象的总线行为。
定义了 6 个不同的对象标志。

|旗帜 |国旗 |适配器使用 ||
| ------------------------- | ------------------------ | ------------------------------------------------- | ---------------------------------------------- |
|C: 通讯标志 | K：通讯标志 |总是设置 ||
|R：读取标志 | L: Les-Flag |对象 native.answer_groupValueResponse ||
|T：传输标志 | Ü: Übertragen 标志 |对象 common.write ||
|W：写标志 | S: Schreiben-Flag |对象 common.read |总线可以修改的对象|
|U：更新标志 | A: Aktualisieren-Flag |对象 common.read |在传入的 GroupValue_Responses 上更新对象 |
|I：初始化标志 | I: Initialisierungs-Flag |对象 native.autoread | |

# 监控和错误跟踪
Openknx 使用 sentry.io 进行应用程序监控和错误跟踪。
它可以帮助开发人员更好地寻找错误并获取现场使用数据。以假名方式跟踪用户的身份。
数据被发送到托管在德国的 Iobroker Sentry 服务器。如果您已允许 iobroker GmbH 收集诊断数据，则还包括您的匿名安装 ID。这允许 Sentry 对错误进行分组并显示有多少唯一用户受到此类错误的影响。

＃ 特征
* 稳定可靠的knx堆栈
* 对最重要的 DPT 的 KNX 数据报进行自动编码/解码，对其他 DPT 进行原始读写
* 支持KNX组值读取和组值写入和组值响应
* 免费开源
* 不依赖云服务，无需互联网访问即可运行
* 开始时自动读取
* 以 XML 格式快速导入群组地址
* 创建对状态输入做出反应的联合别名对象

# 已知问题
- 没有任何

# 限制
- 不支持 ETS 4 导出文件格式
- 不支持 KNX 安全
- 仅支持 IPv4

## Changelog
### 0.1.19 (2022-02-)
* feature:
* bugfix:

### 0.1.18 (2022-01-30)
* bugfix: issue #61 Alias dialog not working 1st time

### 0.1.17 (2022-01-29)
* feature: more information in alias import dialog
* feature: warning on startup if ga are inconsistent
* fix: corrected object count statistics on startup

### 0.1.16 (2022-01-27)
* feature: add back sentry
* fix: stability alias generation
* fix: better input settings plausibilization in admin
* fix: reset after settings change was broken, dont reset for alias change

### 0.1.15 (2022-01-23)
 * feature: more sanity checks for gui
 * feature: issue #84, add openknx to discovery adapter
 * feature: issue #82, warnings on import of duplicate ga addresses, also check iob object for duplicates
 * fix: issue #87, added q interface to trigger GroupValue_Read, comments are overwritten in javascript adapter
 * fix: remove currently unused reference to sentry
 
### 0.1.14 (2022-01-08)
* feature: autodetect the KNX IP interface parameters
* feature: create warning if DPT of alias pair does not match
* feature: create warning in log in case of possible data loss if gateway disconnects
* feature: better gui for import status, newline per warning, count number of succeeding ga's
* fix: local ip interface in admin was not taken
* fix: default regexp for status ga's corrected to match common nomenclature

### 0.1.13 (2021-12-30)
* bugfix: state.value of of type object must be serialized
* bugfix: alias algorithm error handling, takover more info to alias

### 0.1.12 (2021-12-30)
* feature: improve alias status search algorithm, add units
* feature: notify user after import if no dpt subtype is set
* fix: library did not allow to write possible 0 values to certain dpts
* fix: admin dialog ui fixes, better presentation of some warnings

### 0.1.11 (2021-12-28)
* feature: remove more scene DPTs from default autoread
* feature: sends GroupValue_Response on GroupValue_Read if configured
* feature: admin dialog with option to generate aliases (beta)
* feature: admin dialog reactivates after adapter reset
* feature: add support for DPT 7.600
* feature: show logs of knx library
* fix: filter out logs with device address bus interactions
* fix: filter ga names that are forbidden in IOB
* fix: reply with GroupValue_Response on request, not with GroupValue_Write
* fix: remove more scene dpts from autoread

### 0.1.10 (2021-12-24)
* fix: interface to write objects corrected

### 0.1.9 (2021-12-22)
* fix: algorith to generate the iob objects improved
* fix: min max removed for boolean
* fix: ackqnowledgement handling
* removed feature: override path of knx objects
* feature: new logo

### 0.1.8
* (tombox) feature: changed ui and many fixes
* (boellner) feature: skip wrong initial disconnect warning
* (boellner) feature: add translation
* (boellner) doc: github ci pipleline, testing

### 0.1.6
* (boellner) fix: missing dependencies

### 0.1.5
* (boellner) feature: corrected adapter status info.connection (green, yellow, red indicator)
* (boellner) fix: remove default fallback ip settings from stack to get error message on missing configuration
* (boellner) fix: autoread
* (boellner) fix: finding non knx objects int tree leading to problems on startup

### 0.1.3
* (boellner) feature: state roles now set to best match for some elements, default is state
* (boellner) feature: exclude scene dtc (trigger) from autoread
* (boellner) doc: corrected warwings reported by https://adapter-check.iobroker.in/
* (boellner) fix: improve ui of admin dialog
* (boellner) fix: project import, now continue to write iob objects in case of incorrect input file

### 0.1.2
* (boellner) doc: initial test release

### 0.0.19
* (boellner) feature: display warning on ga import file errors

### 0.0.17
* (boellner) feature: raw value handling, can now write and receive ga of unsupported dpt
* (boellner) bugfix: setting onlyAddNewObjects fixed
* (boellner) feature: adapter restart after import

### 0.0.14
* (boellner) feature: import ga xml

### initial version
* initial version inspired by https://www.npmjs.com/package/iobroker.knx/v/0.8.3

## License
					GNU GENERAL PUBLIC LICENSE
==========================  
Copyright Contributors to the ioBroker.openknx project  

					   Version 3, 29 June 2007

 Copyright (C) 2007 Free Software Foundation, Inc. <https://fsf.org/>
 Everyone is permitted to copy and distribute verbatim copies
 of this license document, but changing it is not allowed.

							Preamble

  The GNU General Public License is a free, copyleft license for
software and other kinds of works.

  The licenses for most software and other practical works are designed
to take away your freedom to share and change the works.  By contrast,
the GNU General Public License is intended to guarantee your freedom to
share and change all versions of a program--to make sure it remains free
software for all its users.  We, the Free Software Foundation, use the
GNU General Public License for most of our software; it applies also to
any other work released this way by its authors.  You can apply it to
your programs, too.

  When we speak of free software, we are referring to freedom, not
price.  Our General Public Licenses are designed to make sure that you
have the freedom to distribute copies of free software (and charge for
them if you wish), that you receive source code or can get it if you
want it, that you can change the software or use pieces of it in new
free programs, and that you know you can do these things.

  To protect your rights, we need to prevent others from denying you
these rights or asking you to surrender the rights.  Therefore, you have
certain responsibilities if you distribute copies of the software, or if
you modify it: responsibilities to respect the freedom of others.

  For example, if you distribute copies of such a program, whether
gratis or for a fee, you must pass on to the recipients the same
freedoms that you received.  You must make sure that they, too, receive
or can get the source code.  And you must show them these terms so they
know their rights.

  Developers that use the GNU GPL protect your rights with two steps:
(1) assert copyright on the software, and (2) offer you this License
giving you legal permission to copy, distribute and/or modify it.

  For the developers' and authors' protection, the GPL clearly explains
that there is no warranty for this free software.  For both users' and
authors' sake, the GPL requires that modified versions be marked as
changed, so that their problems will not be attributed erroneously to
authors of previous versions.

  Some devices are designed to deny users access to install or run
modified versions of the software inside them, although the manufacturer
can do so.  This is fundamentally incompatible with the aim of
protecting users' freedom to change the software.  The systematic
pattern of such abuse occurs in the area of products for individuals to
use, which is precisely where it is most unacceptable.  Therefore, we
have designed this version of the GPL to prohibit the practice for those
products.  If such problems arise substantially in other domains, we
stand ready to extend this provision to those domains in future versions
of the GPL, as needed to protect the freedom of users.

  Finally, every program is threatened constantly by software patents.
States should not allow patents to restrict development and use of
software on general-purpose computers, but in those that do, we wish to
avoid the special danger that patents applied to a free program could
make it effectively proprietary.  To prevent this, the GPL assures that
patents cannot be used to render the program non-free.

  The precise terms and conditions for copying, distribution and
modification follow.

					   TERMS AND CONDITIONS

  0. Definitions.

  "This License" refers to version 3 of the GNU General Public License.

  "Copyright" also means copyright-like laws that apply to other kinds of
works, such as semiconductor masks.

  "The Program" refers to any copyrightable work licensed under this
License.  Each licensee is addressed as "you".  "Licensees" and
"recipients" may be individuals or organizations.

  To "modify" a work means to copy from or adapt all or part of the work
in a fashion requiring copyright permission, other than the making of an
exact copy.  The resulting work is called a "modified version" of the
earlier work or a work "based on" the earlier work.

  A "covered work" means either the unmodified Program or a work based
on the Program.

  To "propagate" a work means to do anything with it that, without
permission, would make you directly or secondarily liable for
infringement under applicable copyright law, except executing it on a
computer or modifying a private copy.  Propagation includes copying,
distribution (with or without modification), making available to the
public, and in some countries other activities as well.

  To "convey" a work means any kind of propagation that enables other
parties to make or receive copies.  Mere interaction with a user through
a computer network, with no transfer of a copy, is not conveying.

  An interactive user interface displays "Appropriate Legal Notices"
to the extent that it includes a convenient and prominently visible
feature that (1) displays an appropriate copyright notice, and (2)
tells the user that there is no warranty for the work (except to the
extent that warranties are provided), that licensees may convey the
work under this License, and how to view a copy of this License.  If
the interface presents a list of user commands or options, such as a
menu, a prominent item in the list meets this criterion.

  1. Source Code.

  The "source code" for a work means the preferred form of the work
for making modifications to it.  "Object code" means any non-source
form of a work.

  A "Standard Interface" means an interface that either is an official
standard defined by a recognized standards body, or, in the case of
interfaces specified for a particular programming language, one that
is widely used among developers working in that language.

  The "System Libraries" of an executable work include anything, other
than the work as a whole, that (a) is included in the normal form of
packaging a Major Component, but which is not part of that Major
Component, and (b) serves only to enable use of the work with that
Major Component, or to implement a Standard Interface for which an
implementation is available to the public in source code form.  A
"Major Component", in this context, means a major essential component
(kernel, window system, and so on) of the specific operating system
(if any) on which the executable work runs, or a compiler used to
produce the work, or an object code interpreter used to run it.

  The "Corresponding Source" for a work in object code form means all
the source code needed to generate, install, and (for an executable
work) run the object code and to modify the work, including scripts to
control those activities.  However, it does not include the work's
System Libraries, or general-purpose tools or generally available free
programs which are used unmodified in performing those activities but
which are not part of the work.  For example, Corresponding Source
includes interface definition files associated with source files for
the work, and the source code for shared libraries and dynamically
linked subprograms that the work is specifically designed to require,
such as by intimate data communication or control flow between those
subprograms and other parts of the work.

  The Corresponding Source need not include anything that users
can regenerate automatically from other parts of the Corresponding
Source.

  The Corresponding Source for a work in source code form is that
same work.

  2. Basic Permissions.

  All rights granted under this License are granted for the term of
copyright on the Program, and are irrevocable provided the stated
conditions are met.  This License explicitly affirms your unlimited
permission to run the unmodified Program.  The output from running a
covered work is covered by this License only if the output, given its
content, constitutes a covered work.  This License acknowledges your
rights of fair use or other equivalent, as provided by copyright law.

  You may make, run and propagate covered works that you do not
convey, without conditions so long as your license otherwise remains
in force.  You may convey covered works to others for the sole purpose
of having them make modifications exclusively for you, or provide you
with facilities for running those works, provided that you comply with
the terms of this License in conveying all material for which you do
not control copyright.  Those thus making or running the covered works
for you must do so exclusively on your behalf, under your direction
and control, on terms that prohibit them from making any copies of
your copyrighted material outside their relationship with you.

  Conveying under any other circumstances is permitted solely under
the conditions stated below.  Sublicensing is not allowed; section 10
makes it unnecessary.

  3. Protecting Users' Legal Rights From Anti-Circumvention Law.

  No covered work shall be deemed part of an effective technological
measure under any applicable law fulfilling obligations under article
11 of the WIPO copyright treaty adopted on 20 December 1996, or
similar laws prohibiting or restricting circumvention of such
measures.

  When you convey a covered work, you waive any legal power to forbid
circumvention of technological measures to the extent such circumvention
is effected by exercising rights under this License with respect to
the covered work, and you disclaim any intention to limit operation or
modification of the work as a means of enforcing, against the work's
users, your or third parties' legal rights to forbid circumvention of
technological measures.

  4. Conveying Verbatim Copies.

  You may convey verbatim copies of the Program's source code as you
receive it, in any medium, provided that you conspicuously and
appropriately publish on each copy an appropriate copyright notice;
keep intact all notices stating that this License and any
non-permissive terms added in accord with section 7 apply to the code;
keep intact all notices of the absence of any warranty; and give all
recipients a copy of this License along with the Program.

  You may charge any price or no price for each copy that you convey,
and you may offer support or warranty protection for a fee.

  5. Conveying Modified Source Versions.

  You may convey a work based on the Program, or the modifications to
produce it from the Program, in the form of source code under the
terms of section 4, provided that you also meet all of these conditions:

	a) The work must carry prominent notices stating that you modified
	it, and giving a relevant date.

	b) The work must carry prominent notices stating that it is
	released under this License and any conditions added under section
	7.  This requirement modifies the requirement in section 4 to
	"keep intact all notices".

	c) You must license the entire work, as a whole, under this
	License to anyone who comes into possession of a copy.  This
	License will therefore apply, along with any applicable section 7
	additional terms, to the whole of the work, and all its parts,
	regardless of how they are packaged.  This License gives no
	permission to license the work in any other way, but it does not
	invalidate such permission if you have separately received it.

	d) If the work has interactive user interfaces, each must display
	Appropriate Legal Notices; however, if the Program has interactive
	interfaces that do not display Appropriate Legal Notices, your
	work need not make them do so.

  A compilation of a covered work with other separate and independent
works, which are not by their nature extensions of the covered work,
and which are not combined with it such as to form a larger program,
in or on a volume of a storage or distribution medium, is called an
"aggregate" if the compilation and its resulting copyright are not
used to limit the access or legal rights of the compilation's users
beyond what the individual works permit.  Inclusion of a covered work
in an aggregate does not cause this License to apply to the other
parts of the aggregate.

  6. Conveying Non-Source Forms.

  You may convey a covered work in object code form under the terms
of sections 4 and 5, provided that you also convey the
machine-readable Corresponding Source under the terms of this License,
in one of these ways:

	a) Convey the object code in, or embodied in, a physical product
	(including a physical distribution medium), accompanied by the
	Corresponding Source fixed on a durable physical medium
	customarily used for software interchange.

	b) Convey the object code in, or embodied in, a physical product
	(including a physical distribution medium), accompanied by a
	written offer, valid for at least three years and valid for as
	long as you offer spare parts or customer support for that product
	model, to give anyone who possesses the object code either (1) a
	copy of the Corresponding Source for all the software in the
	product that is covered by this License, on a durable physical
	medium customarily used for software interchange, for a price no
	more than your reasonable cost of physically performing this
	conveying of source, or (2) access to copy the
	Corresponding Source from a network server at no charge.

	c) Convey individual copies of the object code with a copy of the
	written offer to provide the Corresponding Source.  This
	alternative is allowed only occasionally and noncommercially, and
	only if you received the object code with such an offer, in accord
	with subsection 6b.

	d) Convey the object code by offering access from a designated
	place (gratis or for a charge), and offer equivalent access to the
	Corresponding Source in the same way through the same place at no
	further charge.  You need not require recipients to copy the
	Corresponding Source along with the object code.  If the place to
	copy the object code is a network server, the Corresponding Source
	may be on a different server (operated by you or a third party)
	that supports equivalent copying facilities, provided you maintain
	clear directions next to the object code saying where to find the
	Corresponding Source.  Regardless of what server hosts the
	Corresponding Source, you remain obligated to ensure that it is
	available for as long as needed to satisfy these requirements.

	e) Convey the object code using peer-to-peer transmission, provided
	you inform other peers where the object code and Corresponding
	Source of the work are being offered to the general public at no
	charge under subsection 6d.

  A separable portion of the object code, whose source code is excluded
from the Corresponding Source as a System Library, need not be
included in conveying the object code work.

  A "User Product" is either (1) a "consumer product", which means any
tangible personal property which is normally used for personal, family,
or household purposes, or (2) anything designed or sold for incorporation
into a dwelling.  In determining whether a product is a consumer product,
doubtful cases shall be resolved in favor of coverage.  For a particular
product received by a particular user, "normally used" refers to a
typical or common use of that class of product, regardless of the status
of the particular user or of the way in which the particular user
actually uses, or expects or is expected to use, the product.  A product
is a consumer product regardless of whether the product has substantial
commercial, industrial or non-consumer uses, unless such uses represent
the only significant mode of use of the product.

  "Installation Information" for a User Product means any methods,
procedures, authorization keys, or other information required to install
and execute modified versions of a covered work in that User Product from
a modified version of its Corresponding Source.  The information must
suffice to ensure that the continued functioning of the modified object
code is in no case prevented or interfered with solely because
modification has been made.

  If you convey an object code work under this section in, or with, or
specifically for use in, a User Product, and the conveying occurs as
part of a transaction in which the right of possession and use of the
User Product is transferred to the recipient in perpetuity or for a
fixed term (regardless of how the transaction is characterized), the
Corresponding Source conveyed under this section must be accompanied
by the Installation Information.  But this requirement does not apply
if neither you nor any third party retains the ability to install
modified object code on the User Product (for example, the work has
been installed in ROM).

  The requirement to provide Installation Information does not include a
requirement to continue to provide support service, warranty, or updates
for a work that has been modified or installed by the recipient, or for
the User Product in which it has been modified or installed.  Access to a
network may be denied when the modification itself materially and
adversely affects the operation of the network or violates the rules and
protocols for communication across the network.

  Corresponding Source conveyed, and Installation Information provided,
in accord with this section must be in a format that is publicly
documented (and with an implementation available to the public in
source code form), and must require no special password or key for
unpacking, reading or copying.

  7. Additional Terms.

  "Additional permissions" are terms that supplement the terms of this
License by making exceptions from one or more of its conditions.
Additional permissions that are applicable to the entire Program shall
be treated as though they were included in this License, to the extent
that they are valid under applicable law.  If additional permissions
apply only to part of the Program, that part may be used separately
under those permissions, but the entire Program remains governed by
this License without regard to the additional permissions.

  When you convey a copy of a covered work, you may at your option
remove any additional permissions from that copy, or from any part of
it.  (Additional permissions may be written to require their own
removal in certain cases when you modify the work.)  You may place
additional permissions on material, added by you to a covered work,
for which you have or can give appropriate copyright permission.

  Notwithstanding any other provision of this License, for material you
add to a covered work, you may (if authorized by the copyright holders of
that material) supplement the terms of this License with terms:

	a) Disclaiming warranty or limiting liability differently from the
	terms of sections 15 and 16 of this License; or

	b) Requiring preservation of specified reasonable legal notices or
	author attributions in that material or in the Appropriate Legal
	Notices displayed by works containing it; or

	c) Prohibiting misrepresentation of the origin of that material, or
	requiring that modified versions of such material be marked in
	reasonable ways as different from the original version; or

	d) Limiting the use for publicity purposes of names of licensors or
	authors of the material; or

	e) Declining to grant rights under trademark law for use of some
	trade names, trademarks, or service marks; or

	f) Requiring indemnification of licensors and authors of that
	material by anyone who conveys the material (or modified versions of
	it) with contractual assumptions of liability to the recipient, for
	any liability that these contractual assumptions directly impose on
	those licensors and authors.

  All other non-permissive additional terms are considered "further
restrictions" within the meaning of section 10.  If the Program as you
received it, or any part of it, contains a notice stating that it is
governed by this License along with a term that is a further
restriction, you may remove that term.  If a license document contains
a further restriction but permits relicensing or conveying under this
License, you may add to a covered work material governed by the terms
of that license document, provided that the further restriction does
not survive such relicensing or conveying.

  If you add terms to a covered work in accord with this section, you
must place, in the relevant source files, a statement of the
additional terms that apply to those files, or a notice indicating
where to find the applicable terms.

  Additional terms, permissive or non-permissive, may be stated in the
form of a separately written license, or stated as exceptions;
the above requirements apply either way.

  8. Termination.

  You may not propagate or modify a covered work except as expressly
provided under this License.  Any attempt otherwise to propagate or
modify it is void, and will automatically terminate your rights under
this License (including any patent licenses granted under the third
paragraph of section 11).

  However, if you cease all violation of this License, then your
license from a particular copyright holder is reinstated (a)
provisionally, unless and until the copyright holder explicitly and
finally terminates your license, and (b) permanently, if the copyright
holder fails to notify you of the violation by some reasonable means
prior to 60 days after the cessation.

  Moreover, your license from a particular copyright holder is
reinstated permanently if the copyright holder notifies you of the
violation by some reasonable means, this is the first time you have
received notice of violation of this License (for any work) from that
copyright holder, and you cure the violation prior to 30 days after
your receipt of the notice.

  Termination of your rights under this section does not terminate the
licenses of parties who have received copies or rights from you under
this License.  If your rights have been terminated and not permanently
reinstated, you do not qualify to receive new licenses for the same
material under section 10.

  9. Acceptance Not Required for Having Copies.

  You are not required to accept this License in order to receive or
run a copy of the Program.  Ancillary propagation of a covered work
occurring solely as a consequence of using peer-to-peer transmission
to receive a copy likewise does not require acceptance.  However,
nothing other than this License grants you permission to propagate or
modify any covered work.  These actions infringe copyright if you do
not accept this License.  Therefore, by modifying or propagating a
covered work, you indicate your acceptance of this License to do so.

  10. Automatic Licensing of Downstream Recipients.

  Each time you convey a covered work, the recipient automatically
receives a license from the original licensors, to run, modify and
propagate that work, subject to this License.  You are not responsible
for enforcing compliance by third parties with this License.

  An "entity transaction" is a transaction transferring control of an
organization, or substantially all assets of one, or subdividing an
organization, or merging organizations.  If propagation of a covered
work results from an entity transaction, each party to that
transaction who receives a copy of the work also receives whatever
licenses to the work the party's predecessor in interest had or could
give under the previous paragraph, plus a right to possession of the
Corresponding Source of the work from the predecessor in interest, if
the predecessor has it or can get it with reasonable efforts.

  You may not impose any further restrictions on the exercise of the
rights granted or affirmed under this License.  For example, you may
not impose a license fee, royalty, or other charge for exercise of
rights granted under this License, and you may not initiate litigation
(including a cross-claim or counterclaim in a lawsuit) alleging that
any patent claim is infringed by making, using, selling, offering for
sale, or importing the Program or any portion of it.

  11. Patents.

  A "contributor" is a copyright holder who authorizes use under this
License of the Program or a work on which the Program is based.  The
work thus licensed is called the contributor's "contributor version".

  A contributor's "essential patent claims" are all patent claims
owned or controlled by the contributor, whether already acquired or
hereafter acquired, that would be infringed by some manner, permitted
by this License, of making, using, or selling its contributor version,
but do not include claims that would be infringed only as a
consequence of further modification of the contributor version.  For
purposes of this definition, "control" includes the right to grant
patent sublicenses in a manner consistent with the requirements of
this License.

  Each contributor grants you a non-exclusive, worldwide, royalty-free
patent license under the contributor's essential patent claims, to
make, use, sell, offer for sale, import and otherwise run, modify and
propagate the contents of its contributor version.

  In the following three paragraphs, a "patent license" is any express
agreement or commitment, however denominated, not to enforce a patent
(such as an express permission to practice a patent or covenant not to
sue for patent infringement).  To "grant" such a patent license to a
party means to make such an agreement or commitment not to enforce a
patent against the party.

  If you convey a covered work, knowingly relying on a patent license,
and the Corresponding Source of the work is not available for anyone
to copy, free of charge and under the terms of this License, through a
publicly available network server or other readily accessible means,
then you must either (1) cause the Corresponding Source to be so
available, or (2) arrange to deprive yourself of the benefit of the
patent license for this particular work, or (3) arrange, in a manner
consistent with the requirements of this License, to extend the patent
license to downstream recipients.  "Knowingly relying" means you have
actual knowledge that, but for the patent license, your conveying the
covered work in a country, or your recipient's use of the covered work
in a country, would infringe one or more identifiable patents in that
country that you have reason to believe are valid.

  If, pursuant to or in connection with a single transaction or
arrangement, you convey, or propagate by procuring conveyance of, a
covered work, and grant a patent license to some of the parties
receiving the covered work authorizing them to use, propagate, modify
or convey a specific copy of the covered work, then the patent license
you grant is automatically extended to all recipients of the covered
work and works based on it.

  A patent license is "discriminatory" if it does not include within
the scope of its coverage, prohibits the exercise of, or is
conditioned on the non-exercise of one or more of the rights that are
specifically granted under this License.  You may not convey a covered
work if you are a party to an arrangement with a third party that is
in the business of distributing software, under which you make payment
to the third party based on the extent of your activity of conveying
the work, and under which the third party grants, to any of the
parties who would receive the covered work from you, a discriminatory
patent license (a) in connection with copies of the covered work
conveyed by you (or copies made from those copies), or (b) primarily
for and in connection with specific products or compilations that
contain the covered work, unless you entered into that arrangement,
or that patent license was granted, prior to 28 March 2007.

  Nothing in this License shall be construed as excluding or limiting
any implied license or other defenses to infringement that may
otherwise be available to you under applicable patent law.

  12. No Surrender of Others' Freedom.

  If conditions are imposed on you (whether by court order, agreement or
otherwise) that contradict the conditions of this License, they do not
excuse you from the conditions of this License.  If you cannot convey a
covered work so as to satisfy simultaneously your obligations under this
License and any other pertinent obligations, then as a consequence you may
not convey it at all.  For example, if you agree to terms that obligate you
to collect a royalty for further conveying from those to whom you convey
the Program, the only way you could satisfy both those terms and this
License would be to refrain entirely from conveying the Program.

  13. Use with the GNU Affero General Public License.

  Notwithstanding any other provision of this License, you have
permission to link or combine any covered work with a work licensed
under version 3 of the GNU Affero General Public License into a single
combined work, and to convey the resulting work.  The terms of this
License will continue to apply to the part which is the covered work,
but the special requirements of the GNU Affero General Public License,
section 13, concerning interaction through a network will apply to the
combination as such.

  14. Revised Versions of this License.

  The Free Software Foundation may publish revised and/or new versions of
the GNU General Public License from time to time.  Such new versions will
be similar in spirit to the present version, but may differ in detail to
address new problems or concerns.

  Each version is given a distinguishing version number.  If the
Program specifies that a certain numbered version of the GNU General
Public License "or any later version" applies to it, you have the
option of following the terms and conditions either of that numbered
version or of any later version published by the Free Software
Foundation.  If the Program does not specify a version number of the
GNU General Public License, you may choose any version ever published
by the Free Software Foundation.

  If the Program specifies that a proxy can decide which future
versions of the GNU General Public License can be used, that proxy's
public statement of acceptance of a version permanently authorizes you
to choose that version for the Program.

  Later license versions may give you additional or different
permissions.  However, no additional obligations are imposed on any
author or copyright holder as a result of your choosing to follow a
later version.

  15. Disclaimer of Warranty.

  THERE IS NO WARRANTY FOR THE PROGRAM, TO THE EXTENT PERMITTED BY
APPLICABLE LAW.  EXCEPT WHEN OTHERWISE STATED IN WRITING THE COPYRIGHT
HOLDERS AND/OR OTHER PARTIES PROVIDE THE PROGRAM "AS IS" WITHOUT WARRANTY
OF ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING, BUT NOT LIMITED TO,
THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR
PURPOSE.  THE ENTIRE RISK AS TO THE QUALITY AND PERFORMANCE OF THE PROGRAM
IS WITH YOU.  SHOULD THE PROGRAM PROVE DEFECTIVE, YOU ASSUME THE COST OF
ALL NECESSARY SERVICING, REPAIR OR CORRECTION.

  16. Limitation of Liability.

  IN NO EVENT UNLESS REQUIRED BY APPLICABLE LAW OR AGREED TO IN WRITING
WILL ANY COPYRIGHT HOLDER, OR ANY OTHER PARTY WHO MODIFIES AND/OR CONVEYS
THE PROGRAM AS PERMITTED ABOVE, BE LIABLE TO YOU FOR DAMAGES, INCLUDING ANY
GENERAL, SPECIAL, INCIDENTAL OR CONSEQUENTIAL DAMAGES ARISING OUT OF THE
USE OR INABILITY TO USE THE PROGRAM (INCLUDING BUT NOT LIMITED TO LOSS OF
DATA OR DATA BEING RENDERED INACCURATE OR LOSSES SUSTAINED BY YOU OR THIRD
PARTIES OR A FAILURE OF THE PROGRAM TO OPERATE WITH ANY OTHER PROGRAMS),
EVEN IF SUCH HOLDER OR OTHER PARTY HAS BEEN ADVISED OF THE POSSIBILITY OF
SUCH DAMAGES.

  17. Interpretation of Sections 15 and 16.

  If the disclaimer of warranty and limitation of liability provided
above cannot be given local legal effect according to their terms,
reviewing courts shall apply local law that most closely approximates
an absolute waiver of all civil liability in connection with the
Program, unless a warranty or assumption of liability accompanies a
copy of the Program in return for a fee.

					 END OF TERMS AND CONDITIONS

			How to Apply These Terms to Your New Programs

  If you develop a new program, and you want it to be of the greatest
possible use to the public, the best way to achieve this is to make it
free software which everyone can redistribute and change under these terms.

  To do so, attach the following notices to the program.  It is safest
to attach them to the start of each source file to most effectively
state the exclusion of warranty; and each file should have at least
the "copyright" line and a pointer to where the full notice is found.

	<one line to give the program's name and a brief idea of what it does.>
	Copyright (C) <year>  <name of author>

	This program is free software: you can redistribute it and/or modify
	it under the terms of the GNU General Public License as published by
	the Free Software Foundation, either version 3 of the License, or
	(at your option) any later version.

	This program is distributed in the hope that it will be useful,
	but WITHOUT ANY WARRANTY; without even the implied warranty of
	MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
	GNU General Public License for more details.

	You should have received a copy of the GNU General Public License
	along with this program.  If not, see <https://www.gnu.org/licenses/>.

Also add information on how to contact you by electronic and paper mail.

  If the program does terminal interaction, make it output a short
notice like this when it starts in an interactive mode:

	<program>  Copyright (C) <year>  <name of author>
	This program comes with ABSOLUTELY NO WARRANTY; for details type `show w'.
	This is free software, and you are welcome to redistribute it
	under certain conditions; type `show c' for details.

The hypothetical commands `show w' and `show c' should show the appropriate
parts of the General Public License.  Of course, your program's commands
might be different; for a GUI interface, you would use an "about box".

  You should also get your employer (if you work as a programmer) or school,
if any, to sign a "copyright disclaimer" for the program, if necessary.
For more information on this, and how to apply and follow the GNU GPL, see
<https://www.gnu.org/licenses/>.

  The GNU General Public License does not permit incorporating your program
into proprietary programs.  If your program is a subroutine library, you
may consider it more useful to permit linking proprietary applications with
the library.  If this is what you want to do, use the GNU Lesser General
Public License instead of this License.  But first, please read
<https://www.gnu.org/licenses/why-not-lgpl.html>.