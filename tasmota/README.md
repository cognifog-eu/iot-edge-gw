# Tasmota configuration templates
This section includes a set of template files to preconfigure ESP32-S2 devices based on the Tasmota firmware.

*(Include information here on how to restore a configuration file into a fresh ESP32-S2 device)*

## A. Config_tasmota_069412_5138_13.3.0.dmp
Minimal configuration of an ESP32-S2 for Tasmota 13.3.0, created at 04/01/2024 with the following features:
- Device Name (*Model* field in Home Assistant menu): `ESP32`
- Friendly Name (*Device* field in Home Assistant menu): `Tasmota1`
- Wifi connectivity:
  - SSID: `iot_responda`
  - Password: `<protected>`
- Sensors:
  - 1 RGB led sensor (WS2812) in GPIO18
  - 1 Temperature + humidity sensor (DHT22; mapped in Tasmota as AM2301) in GPIO17
- Teleperiod (seconds): `30`
- Matter compatibility with 3 predefined endpoints (see attached table)  

| Endpoint   | Type            | Parameter            |
|------------|-----------------|----------------------|
|     1      | Light 3 RGB     | *_Not used_*         |
|     2      | Temperature     | AM2301#Temperature   |
|     3      | Humidity        | AM2301#Humidity      |