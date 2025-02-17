---
BADGE-Number of Installations: http://iobroker.live/badges/zigbee-stable.svg
BADGE-NPM version: http://img.shields.io/npm/v/iobroker.zigbee.svg
BADGE-Downloads: https://img.shields.io/npm/dm/iobroker.zigbee.svg
---
# ioBroker Adapter für Zigbee-Geräte
Mit Hilfe eines Koordinators für Zigbee-Netz, basierend auf Texas Instruments SoC cc253x (und anderen), wird ein eigenes Netz erschaffen, welchem sich andere Zigbee Geräte beitreten können. Dank der direkten Interaktion mit dem Koordinator, erlaubt der Zigbee Adapter die Steuerung der Geräte ohne jegliche Gateways/Bridges der Hersteller (Xiaomi/Tradfri/Hue). Über Funktionsweise der Zigbee-Netze kann man [hier nachlesen (Englisch)](https://github.com/Koenkk/zigbee2mqtt/wiki/ZigBee-network).

## Die Hardware
Für die Umsetzung wird einer der aufgezählten Geräte/Sticks verwendet, welche mit spezieller ZNP-Firmware geflasht sind: [cc2530, cc2530, cc2530+RF.](https://github.com/Koenkk/zigbee2mqtt/wiki/Supported-sniffer-devices#zigbee-coordinator)

![](img/CC2531.png)
![](img/sku_429478_2.png)
![](img/sku_429601_2.png)
![](img/CC2591.png)

Der benötigte Flasher/Programmer und der Prozess der Vorbereitung werden [hier (Englisch)](https://github.com/Koenkk/zigbee2mqtt/wiki/Getting-started) oder [hier (Russisch)](https://github.com/kirovilya/ioBroker.zigbee/wiki/%D0%9F%D1%80%D0%BE%D1%88%D0%B8%D0%B2%D0%BA%D0%B0) beschrieben. 

Die mit dem Zigbee-Netz verbundenen Geräte übermitteln dem Koordinator ihren Zustand und benachrichtigen über Ereignisse (Knopfdruck, Bewegungserkennung, Temperaturänderung). Diese Infos werden im Adapter unter den jeweiligen Objekten angezeigt. Außerdem ist es möglich manche Ereignisse/Status zurück zum Zigbee-Gerät zusenden (Zustandsänderung Steckdosen und Lampen, Farb- und Helligkeitseinstellungen).

## Einstellungen und Pairing
![](https://raw.githubusercontent.com/kirovilya/files/master/config.PNG)

Zu Beginn muss der USB-Port angegeben werden, an welchem der cc253x angeschlossen ist. Wie man diesen Erkennt ist [hier beschrieben (Russisch)](https://github.com/kirovilya/ioBroker.zigbee/wiki#%D0%9D%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B0-%D0%B0%D0%B4%D0%B0%D0%BF%D1%82%D0%B5%D1%80%D0%B0)

Zum Verbinden der Geräte muss der Koordinator für Zigbee-Netz in den Pairingmodus versetzt werden, dazu auf den grünen Knopf im Adapter klicken. Pairingmodus ist ab jetzt für 60 Sekunden aktiv. Um die Geräte zu verbinden, reicht im Normallfall ein Betätigen des Knopfes auf dem zu verbindendem Gerät. Es gibt aber auch „besondere“ Geräte. Wie man diese verbindet ist [hier Englisch](https://github.com/Koenkk/zigbee2mqtt/wiki/Pairing-devices) [oder Russisch](https://github.com/kirovilya/ioBroker.zigbee/wiki#%D0%9F%D0%BE%D0%B4%D0%B4%D0%B5%D1%80%D0%B6%D0%B8%D0%B2%D0%B0%D0%B5%D0%BC%D1%8B%D0%B5-%D1%83%D1%81%D1%82%D1%80%D0%BE%D0%B9%D1%81%D1%82%D0%B2%D0%B0) beschrieben.

Nach erfolgreichem Pairing, wird das Gerät im Adapter angezeigt. Sollte ein Gerät (aus der Liste) den Namen „undefined“ haben, dann versucht es zu löschen und nochmal zu pairen. Sollte es trotzdem nicht funktionieren, schreibt bitte ein Issue.
Zigbee-Geräte die nicht in der Liste aufgeführt sind, können zwar gepairt werden, aber der Adapter kann mit diesen nicht kommunizieren.

## Zusätzliche Informationen
Es gibt noch ein [Freundschaftprojekt](https://github.com/koenkk/zigbee2mqtt) mit gleichen Funktionen und gleicher Technologie, welcher mit denselben Geräten über ein MQTT Protokoll kommuniziert. Wenn irgendwelche Verbesserungen oder neu unterstütze Geräte im Projekt Zigbee2MQTT eingefügt werden, können jene auch in dieses Projekt hinzugefügt werden. Solltet Ihr unterschiede merken, schreibt bitte ein Issue, wir kümmern uns darum

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