# ESP Gas Connect BK

ESPHome firmware for the **ESP Gas Connect BK** by [NG3D](https://ng3d-druck.de/products/reedkontakt-sensor-bk-gaszaehler-esp32):
a reed contact pulse sensor for BK gas meters with a built-in Waveshare ESP32-C3-Zero.

## Hardware

| Pin    | Function                          |
| ------ | --------------------------------- |
| GPIO4  | Reed contact (to GND, pull-up)    |
| GPIO10 | Onboard WS2812 RGB LED            |

One revolution of the last drum digit of a BK meter equals 0.01 m³, so each pulse counts 0.01 m³.

## Entities

- **Gas meter reading** (m³, `total_increasing`, device class `gas`) - for the Home Assistant energy dashboard
- **Gas flow** (m³/h)
- **Gas today**, **Gas this month**, **Gas this year** (m³) - consumption since local midnight / the 1st of the month / January 1st
- **Gas yesterday** (m³) - consumption of the previous day (unknown if the device was off for a whole day)
- **Set meter reading** - enter the value shown on your meter once, it is persisted across reboots
- **Reed contact** (diagnostic) - live contact state, useful when aligning the sensor
- **LED**, **Restart**, **WiFi signal**, **Uptime**, **Firmware** update

## Setup

1. Power the device via USB-C.
2. Connect it to your Wi-Fi with Improv via Bluetooth (Home Assistant / ESPHome app),
   Improv via USB at the [web installer](https://ng3d-druck.github.io/Gas-Connect-for-ESPHome/), or the fallback hotspot.
3. Adopt it in Home Assistant and set **Set meter reading** to the current value of your meter.

The periods roll over at local midnight, using SNTP and the `timezone` substitution (default `Europe/Berlin`).

## German entity names

ESPHome compiles entity names into the firmware, so they can't be switched at runtime. To use German names,
take control of the device in the ESPHome Device Builder and add this to your config:

```yaml
substitutions:
  name_gas_flow: Gasdurchfluss
  name_gas_meter_reading: Gaszählerstand
  name_gas_today: Gas heute
  name_gas_yesterday: Gas gestern
  name_gas_this_month: Gas diesen Monat
  name_gas_this_year: Gas dieses Jahr
  name_set_meter_reading: Zählerstand setzen
  name_reed_contact: Reedkontakt
  name_wifi_signal: WLAN-Signal
  name_uptime: Laufzeit
  name_led: LED
  name_restart: Neustart
```

Renaming changes the entity IDs, so Home Assistant creates new entities and the history of the old ones
(including the energy dashboard) is not carried over. Rename before setting up the energy dashboard, or
just rename the entities in Home Assistant instead.

## Files

- `esp-gas-connect-bk.yaml` - the configuration you get when taking control in the ESPHome Device Builder
- `esp-gas-connect-bk.factory.yaml` - the factory firmware (adds Improv, project info and OTA updates)
