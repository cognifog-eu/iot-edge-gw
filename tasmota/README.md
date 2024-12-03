# Configuration of MATTER-based devices via Tasmota
End devices can be configured to interact with the IoT Edge Gateway by means of the MATTER protocol. More specifically, this communication has been validated by using ESP32-S2 DevKitM-1 devices (https://docs.espressif.com/projects/esp-idf/en/latest/esp32s2/hw-reference/esp32s2/user-guide-devkitm-1-v1.html) and the firmware provided by Tasmota (https://tasmota.github.io/docs/).

Tasmota firmware can be loaded to ESP32-based devices by means of its web installer: https://tasmota.github.io/install/. It is only necessary to previously connect the ESP32-based device to a laptop via USB.

Alternatively, Tasmota pre-configured files with the configuration of Tasmota 13.3.0 and 14.3.0 for an ESP32-S2 DevKitM-1 device have been validated and are provided in this folder. To flash an ESP32-S2 device with this firmware, connect the ESP32-S2 to a laptop via USB and visit its web installer: https://tasmota.github.io/install/. Then, follow these steps:
1. Click 'Upload factory.bin' and select the file `tasmota32s2_13.3.0.factory.bin`. Alternatively, all release binaries for Tasmota firmware 13.3.0 on ESP32 can be found at https://ota.tasmota.com/tasmota32/release-13.3.0/ 
2. Press 'CONNECT' and select the COM port to which the device is connected. Setup will execute automatically
3. Once the installation is finished, configure the WiFi network appropiately
4. Then navigate with the browser to the IP allocated to the ESP32-S2 and click 'Configuration -> Restore Configuration'
5. Use the file provided in this folder with the format `Config_tasmota_<version>.dmp` and click 'Start restore'. Please note that WiFi connections are also preconfigured in the configuration file. In consequence, Tasmota configuration can only be modified by accessing its IP address in the corresponding WiFi network 
6. Once the restoration is completed, change the Friendly Name of the device. To do it, go to 'Configuration -> Configure Other' and change the `Device Name` and the `Friendly Name` fields to `Tasmota<X>`, being `<X>` a unique value between `1` and `10`
7. Since the pre-configured file includes the compatibility with MATTER, a 10-minute window is automatically opened to perform the commissioning process with a MATTER controller. Go to the 'Main Menu' and scan the QR code with the 'Home Assistant mobile app' installed in a mobile device. **To complete the commissioning process successfully, all three elements (i.e., the MATTER-based device, the IoT Edge GW, and the phone running the mobile app) must be in the same network.**
  - Android: https://play.google.com/store/apps/details?id=io.homeassistant.companion.android
  - iOS: https://apps.apple.com/us/app/home-assistant/id1099568401

![alt text](img/commissioning.png)

8. If successful, the 'Home Assistant mobile app' should display a message similar to 'Commissioning complete'

Apart from the Web UI interface of Tasmota, there are also alternative ways to communicate with these devices: via MQTT, via web requests, and over serial bridge (see https://tasmota.github.io/docs/Commands/ for more information). As a matter of example, an easy way to retrieve the basic information of the device flashed with the Tasmota firmware via a web request is the following:

`curl "http://<IP_address_of_the_Tasmota_device>/cm?cmnd=Status%200"`

## A. Config_tasmota_064876_2166_13.3.0.dmp
Minimal configuration of an ESP32-S2 for Tasmota 13.3.0, created at 23/04/2024 with the following features:
- Device Name (*Model* field in Home Assistant menu): `Tasmota1` (please change the name to `Tasmota<X>` as previously mentioned)
- Friendly Name (*Device* field in Home Assistant menu): `Tasmota1` (please change the name to `Tasmota<X>` as previously mentioned)
- Wifi connectivity based on 1 preconfigured network:
  - SSID_1: `cognifog_wifi`
  - Password: `<protected>`
- Sensors:
  - 1 RGB led sensor (WS2812) in GPIO18
  - 1 Temperature + humidity sensor (DHT22; mapped in Tasmota as AM2301) in GPIO17
- Teleperiod (i.e., sensor data update rate) in seconds: `30`
- Matter compatibility with 3 predefined endpoints (see attached table)  

| Endpoint   | Type            | Parameter            |
|------------|-----------------|----------------------|
|     1      | Light 3 RGB     | *_Not used_*         |
|     2      | Temperature     | AM2301#Temperature   |
|     3      | Humidity        | AM2301#Humidity      |

## B. Config_tasmota_064876_2166_14.3.0
Minimal configuration of an ESP32-S2 for Tasmota 14.3.0, created at 03/12/2024 with the following features:
- Device Name (*Model* field in Home Assistant menu): `Tasmota` (please change the name to `Tasmota<X>` as previously mentioned)
- Friendly Name (*Device* field in Home Assistant menu): `Tasmota` (please change the name to `Tasmota<X>` as previously mentioned)
- Inexplicably, when Device and Friendly Names are changed in Tasmota 14.3.0, previously loaded configuration of Matter endpoints is lost. So it is necessary to configure them again
- Wifi connectivity based on 1 preconfigured network:
  - SSID_1: `cognifog_wifi`
  - Password: `<protected>`
- Sensors:
  - 1 RGB led sensor (WS2812) in GPIO18
  - 1 Temperature + humidity sensor (DHT22; mapped in Tasmota as AM2301) in GPIO17
- Teleperiod (i.e., sensor data update rate) in seconds: `30`
- Matter compatibility with 3 predefined endpoints (see attached table)  

| Endpoint   | Type            | Parameter            |
|------------|-----------------|----------------------|
|     2      | Light 3 RGB     | *_Not used_*         |
|     3      | Temperature     | AM2301#Temperature   |
|     4      | Humidity        | AM2301#Humidity      |