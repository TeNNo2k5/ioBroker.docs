---
BADGE-Number of Installations: http://iobroker.live/badges/zigbee-stable.svg
BADGE-NPM version: http://img.shields.io/npm/v/iobroker.zigbee.svg
BADGE-Downloads: https://img.shields.io/npm/dm/iobroker.zigbee.svg
---
# Драйвер ioBroker для работы с Zigbee-устройствами
При помощи координатора Zigbee-сети на базе Texas Instruments SoC cc253x (и другими) создается собственная сеть, в которую подключаются zigbee-устройства. Взаимодействуя напрямую с координатором сети, драйвер позволяет управлять устройствами без дополнительных шлюзов/бриджей от производителей устройств (Xiaomi/TRADFRI/Hue). Про устройство Zigbee-сети можно прочитать [тут (на английском языке)](https://github.com/Koenkk/zigbee2mqtt/wiki/ZigBee-network). 

## Оборудование
Для работы необходимо одно из перечисленных устройств, прошитое специальной ZNP-прошивкой: [cc2531, cc2530, cc2530+RF](https://github.com/Koenkk/zigbee2mqtt/wiki/Supported-sniffer-devices#zigbee-coordinator)

![](img/CC2531.png)
![](img/sku_429478_2.png)
![](img/sku_429601_2.png)
![](img/CC2591.png)
Необходимое для прошивки оборудование и процесс подготовки устройства описан [тут (на английском языке)](https://github.com/Koenkk/zigbee2mqtt/wiki/Getting-started) или [тут (на русском языке)](https://github.com/kirovilya/ioBroker.zigbee/wiki/%D0%9F%D1%80%D0%BE%D1%88%D0%B8%D0%B2%D0%BA%D0%B0) 

Подключенные к Zigbee-сети устройства сообщают координатору своё состояние и информируют о событиях (нажатия кнопки, обнаружение движения, изменение температуры). Эти сведения отражаются в виде объектов-состояний ioBroker. Некоторые состояния имеют обратную связь и могут отправлять команды zigbee-устройству при изменении состояния (переключение состояния розетки и лампы, изменение сцены или яркости лампы).

## Работа с драйвером
Для запуска драйвера необходимо указать имя порта, на котором подключено устройство cc253x.
Как узнать порт написано [тут (на русском языке)](https://github.com/kirovilya/ioBroker.zigbee/wiki#%D0%9D%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B0-%D0%B0%D0%B4%D0%B0%D0%BF%D1%82%D0%B5%D1%80%D0%B0)

Для подключения устройств необходимо перевести координатор Zigbee-сети в режим сопряжения, нажав зеленую кнопку. Начнется обратный отсчет времени (60 сек) пока будет доступна возможность подключения устройств.
Для подключения Zigbee-устройств в большинстве случаев достаточно нажать кнопку сопряжения на самом устройстве. Но существуют особенности для некоторых устройств. Подробнее о сопряжении с устройствами читайте [тут (на английском языке)](https://github.com/Koenkk/zigbee2mqtt/wiki/Pairing-devices) или [тут (на русском языке)](https://github.com/kirovilya/ioBroker.zigbee/wiki#%D0%9F%D0%BE%D0%B4%D0%B4%D0%B5%D1%80%D0%B6%D0%B8%D0%B2%D0%B0%D0%B5%D0%BC%D1%8B%D0%B5-%D1%83%D1%81%D1%82%D1%80%D0%BE%D0%B9%D1%81%D1%82%D0%B2%D0%B0).

После успешного сопряжения, устройство появится в панели устройств. Если устройство появилось в панели устройств, но имеет тип undefined, то это неизвестное устройство и с ним нельзя будет взаимодействовать. Если устройство есть в списке доступных устройств, но добавилось как undefined, то попробуйте удалить устройство и добавить заново.
Если не получается подключить устройство, то напиши issue.

## Дополнительные сведения
Существует [дружественный проект](https://github.com/koenkk/zigbee2mqtt) со схожим функционалом на тех же технологиях, в котором с этими же устройствами можно работать по протоколу MQTT.
Поэтому, если какие-либо улучшения или поддержка новых zigbee-устройств происходит в проекте Zigbee2MQTT, то можно перенести и добавить этот же функционал в этот драйвер. Если заметили это, то напиши issue - перенесем.

## Changelog
### 1.6.14 (2022-01)
* (asgothian) OTA limitation
  - devices with the available state set to false are excluded from OTA updates (and the update check)
  - devices with link_quality 0 are excluded from OTA updates (and the update check)
* (asgothian) Device deactivation:
  - Devices can be marked inactive from the device card.
  - inactive devices are not pinged
  - state changes by the user are not sent to inactive devices.
  - when a pingable device is marked active (from being inactive) it will be pinged again.
  - inactive devices are excluded from OTA updates.
* (asgothian) Group rework part 2:
  - state device.groups will now be deleted with state Cleanup
  - state info.groups is now obsolete and will be deleted at adapter start (after transferring data to
    the new storage)
* (asgothian) Device name persistance.
  - Changes to device names made within the zigbee adapter are stored in the file dev_names.json. This file
    is not deleted when the adapter is removed, and will be referenced when a device is added to the zigbee adapter. Deleting and reinstalling the adapter will no longer remove custom device names, nor will deleting and adding the device anew.
* (asgothian) Readme edit to reflect the current information on zigbee coordinator hardware.
* (arteck) Zigbee-Herdsman 0.14.4, Zigbee-Herdsman-Converters 14.0.394

### 1.6.13 (2022-01)

* (kirovilya) update to Zigbee-Herdsman 0.14


### 1.6.12 (2022-01)
* (asgothian) Groups were newly revised (read [here](https://github.com/ioBroker/ioBroker.zigbee/pull/1327) )
   -  object device.groups is obsolet..the old one is no longer up to date


### 1.6.9 (2021-12)
* (simatec) fix admin Dark-Mode
* (asgothian) Expose Access Handling
* (arteck) translations
* (asgothian) fix groups
* (agross) use different normalization rules

### 1.6.1 (2021-08)
* (kirovilya) herdsman compatibility

### 1.6.0 (2021-08-09)

## License
The MIT License (MIT)

Copyright (c) 2018-2021 Kirov Ilya <kirovilya@gmail.com>

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.