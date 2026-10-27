# OpenInverterGateway für Growatt SPH TL3 BH-UP (mit BMS-Werten)

Angepasste Version von [OpenInverterGateway](https://github.com/OpenInverterGateway/OpenInverterGateway)
(Basis: Commit `a35045e`, Mai 2026) für einen **ShineWiFi-X**-Stick am USB-Anschluss eines
**Growatt SPH 4000–10000TL3 BH-UP**.

Getestet mit: Growatt SPH 10000TL3 BH-UP + umgeflashter ShineWiFi-X.

## Was ist anders als im Original?

- Modbus-Protokoll **1.24** (`GROWATT_MODBUS_VERSION 124`) – SPH nutzt laut Growatt-Dokumentation
  die Register 0–124 und 1000–1124 (nicht 3000+ wie TL-XH).
- Zusätzlicher Lesebereich **1086–1096** (BMS-Daten aus „Growatt Inverter Modbus RTU Protocol II V1.24“):

| Register | Name in JSON/MQTT        | Einheit | Faktor |
|----------|--------------------------|---------|--------|
| 1086     | `BMSStateOfCharge`       | %       | 1      |
| 1087     | `BMSBatteryVoltage`      | V       | 0,01   |
| 1088     | `BMSBatteryCurrent`      | A       | 0,01   |
| 1089     | `BMSBatteryTemperature`  | °C      | 0,1    |
| 1095     | `BMSCycleCount`          | –       | 1      |
| 1096     | `BMSStateOfHealth`       | %       | 1      |

Geänderte Dateien: `SRC/ShineWiFi-ModBus/Growatt124.cpp` und `Growatt124.h`
(siehe auch `sph-bms-register.patch`).

## Fertige Firmware (Release)

| Datei | Inhalt |
|-------|--------|
| `OpenInverterGateway_SPH_Protokoll124.bin` | Original-Code, nur Protokoll 1.24 |
| `OpenInverterGateway_SPH_124_BMS.bin`      | Protokoll 1.24 + BMS-Register |

Gebaut mit ESP8266-Arduino-Core 3.1.2, Board „Generic ESP8266“, 4 MB (FS 1 MB), Flash-Mode DOUT.
Prüfsummen: `SHA256SUMS.txt`.

## Einspielen (Stick läuft schon mit OpenInverterGateway)

1. Config-Access-Point starten (Weboberfläche → „Start Config AP“ oder AP-Taste am Stick).
2. Mit dem WLAN „GrowattConfig“ verbinden, http://192.168.4.1 öffnen.
3. „Update“ → `.bin` auswählen → „Update“. Stick währenddessen nicht abziehen.

Ein Original-Growatt-Stick muss beim ersten Mal per Kabel/Seriell geflasht werden –
siehe Doku im Original-Projekt.

## Hinweise

- Mit dieser Firmware sendet der Stick **keine Daten an die Growatt-Cloud** (App bleibt leer).
- Einspielen auf eigene Gefahr. Keine Gewähr.

## Lizenz

MIT – wie das Original. Copyright (c) 2019 otti und Mitwirkende von OpenInverterGateway.
