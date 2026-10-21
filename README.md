# Gas Connect for ESPHome

[Deutsch](#deutsch) | [English](#english)

## Deutsch

ESPHome-Firmware für den **Gas Connect** von [NG3D-Druck](https://ng3d-druck.de):
ein Impulssensor für Gaszähler mit Reedkontakt, mit eingebautem Waveshare ESP32-C3-Zero.

### Hardware

| Pin    | Funktion                          |
| ------ | --------------------------------- |
| GPIO4  | Reedkontakt (gegen GND, Pull-up)  |
| GPIO10 | WS2812-RGB-LED auf der Platine    |

Jede Umdrehung der letzten Rollenziffer erzeugt einen Impuls. Wie viel Gas das ist, steht auf dem Typenschild des Zählers
(bei den meisten Gaszählern 0,01 m³) und wird über **Pulse volume** eingestellt.

### Entitäten

- **Gas meter reading** (Zählerstand, m³, `total_increasing`, Geräteklasse `gas`) - für das Energie-Dashboard von Home Assistant
- **Gas flow** (Durchfluss, m³/h)
- **Gas today**, **Gas this month**, **Gas this year** (m³) - Verbrauch seit Mitternacht (Ortszeit) / dem 1. des Monats / dem 1. Januar
- **Gas yesterday** (m³) - Verbrauch des Vortags (unbekannt, wenn das Gerät einen ganzen Tag aus war)
- **Set meter reading** - einmal den Stand vom Zähler eintragen, er bleibt über Neustarts erhalten
- **Pulse volume** - Volumen pro Impuls (0,001 / 0,01 / 0,1 / 1 m³, Standard 0,01 m³); beim Umstellen bleiben Zählerstand und Verbrauch erhalten
- **Debounce time** - Entprellzeit des Reedkontakts (10-1000 ms, Standard 100 ms); erhöhen, wenn ein Impuls doppelt gezählt wird,
  verringern, wenn bei hohem Durchfluss Impulse fehlen
- **Reed contact** (Diagnose) - aktueller Zustand des Kontakts, hilfreich beim Ausrichten des Sensors
- **LED mode** - was die LED anzeigt: **Status + pulse** (Standard), **Status only** oder **Off**
- **Restart**, **WiFi signal**, **Uptime**
- **Firmware**-Update (nur in der Werks-Firmware, entfällt nach der Übernahme im ESPHome Device Builder)

### LED

| LED                    | Bedeutung                                         |
| ---------------------- | ------------------------------------------------- |
| blau, blinkend         | kein WLAN (Hotspot bzw. Improv aktiv)             |
| gelb, blinkend         | WLAN verbunden, aber keine Verbindung zu Home Assistant |
| kurz grün              | Impuls erkannt (nur bei **Status + pulse**)       |
| aus                    | alles in Ordnung                                  |

### Einrichtung

1. Das Gerät über USB-C mit Strom versorgen.
2. Mit dem WLAN verbinden: per Improv über Bluetooth (Home Assistant / ESPHome-App),
   per Improv über USB im [Web-Installer](https://ng3d-druck.github.io/Gas-Connect-for-ESPHome/) oder über den Fallback-Hotspot.
3. In Home Assistant übernehmen (es erscheint als `gas-connect-xxxxxx`, die Endung sind die letzten Stellen der MAC-Adresse)
   und bei **Set meter reading** den aktuellen Stand des Zählers eintragen.
   Falls dein Zähler nicht 0,01 m³ pro Umdrehung hat, vorher **Pulse volume** anpassen.

Die Zeiträume wechseln um Mitternacht Ortszeit. Die Zeitzone übernimmt das Gerät automatisch aus den Einstellungen von
Home Assistant (ab Version 2026.3). Bis Home Assistant verbunden ist, oder bei älteren Versionen, gilt `Europe/Berlin`
(Substitution `timezone`).

### Dateien

- `gas-connect.yaml` - die Konfiguration, die man bei der Übernahme im ESPHome Device Builder erhält
- `gas-connect.factory.yaml` - die Werks-Firmware (ergänzt Improv, Projektinfos und OTA-Updates)

## English

ESPHome firmware for the **Gas Connect** by [NG3D-Druck](https://ng3d-druck.de):
a pulse sensor for gas meters with a reed contact, with a built-in Waveshare ESP32-C3-Zero.

### Hardware

| Pin    | Function                          |
| ------ | --------------------------------- |
| GPIO4  | Reed contact (to GND, pull-up)    |
| GPIO10 | Onboard WS2812 RGB LED            |

Each revolution of the last drum digit creates one pulse. How much gas that is is printed on the meter
(0.01 m³ for most gas meters) and is set via **Pulse volume**.

### Entities

- **Gas meter reading** (m³, `total_increasing`, device class `gas`) - for the Home Assistant energy dashboard
- **Gas flow** (m³/h)
- **Gas today**, **Gas this month**, **Gas this year** (m³) - consumption since local midnight / the 1st of the month / January 1st
- **Gas yesterday** (m³) - consumption of the previous day (unknown if the device was off for a whole day)
- **Set meter reading** - enter the value shown on your meter once, it is persisted across reboots
- **Pulse volume** - volume per pulse (0.001 / 0.01 / 0.1 / 1 m³, default 0.01 m³); changing it keeps the meter reading and consumption
- **Debounce time** - debounce time of the reed contact (10-1000 ms, default 100 ms); raise it if a pulse is counted twice,
  lower it if pulses are missed at high flow
- **Reed contact** (diagnostic) - live contact state, useful when aligning the sensor
- **LED mode** - what the LED shows: **Status + pulse** (default), **Status only** or **Off**
- **Restart**, **WiFi signal**, **Uptime**
- **Firmware** update (factory firmware only, gone after taking control in the ESPHome Device Builder)

### LED

| LED                    | Meaning                                           |
| ---------------------- | ------------------------------------------------- |
| blue, blinking         | no Wi-Fi (hotspot or Improv active)               |
| yellow, blinking       | Wi-Fi connected, but no Home Assistant connection |
| short green flash      | pulse detected (only with **Status + pulse**)     |
| off                    | all fine                                          |

### Setup

1. Power the device via USB-C.
2. Connect it to your Wi-Fi with Improv via Bluetooth (Home Assistant / ESPHome app),
   Improv via USB at the [web installer](https://ng3d-druck.github.io/Gas-Connect-for-ESPHome/), or the fallback hotspot.
3. Adopt it in Home Assistant (it shows up as `gas-connect-xxxxxx`, the suffix being the end of its MAC address)
   and set **Set meter reading** to the current value of your meter.
   If your meter does not count 0.01 m³ per revolution, set **Pulse volume** first.

The periods roll over at local midnight. The device takes the timezone from the Home Assistant settings automatically
(Home Assistant 2026.3 and newer). Until Home Assistant is connected, or with older versions, `Europe/Berlin` applies
(`timezone` substitution).

### Files

- `gas-connect.yaml` - the configuration you get when taking control in the ESPHome Device Builder
- `gas-connect.factory.yaml` - the factory firmware (adds Improv, project info and OTA updates)
