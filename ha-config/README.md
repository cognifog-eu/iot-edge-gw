# Home Assistant configuration
This section includes a set of template files to preconfigure Home Assistant according to the recommendations provided in the section **Migrating to a new system** from https://www.home-assistant.io/docs/configuration/.
For the COGNIFOG project, the configuration of Home Assistant in a fresh new IoT Edge Gateway is based on 3 steps:
1. To apply a new basic Home Assistant configuration, copy the `ha-config-<version>.rar` file into the Raspberry Pi, unzip it, and paste it into `/home/cognifog/iot-edge-gw/ha-config`, replacing all previously existing files.
2. Then, set the basic information of the IoT Edge Gateway according to the template provided in the file `configuration.yaml` of this repository and replace the one existing in the folder `/home/cognifog/iot-edge-gw/ha-config` of the Raspberry Pi.
3. Similarly, copy the file `automations.yaml` of this repository and replace the one existing in the folder `/home/cognifog/iot-edge-gw/ha-config` of the Raspberry Pi. This file contains all automations that enable the non-assisted operation of Home Assistant.

## A. ha-config-v1.0.rar
Minimal configuration of Home Assistant, with the following configured capabilities:
- Integration with the MATTER controller add-on for Home Assistant
- Configuration of an MQTT client in Home Assistant that can be connected to an external MQTT broker with the following features:
  - MQTT broker IP address: `84.88.36.141`
  - MQTT broker port: `1883`
  - MQTT client username: `i2cat`
  - MQTT client password: `<protected>`
  - 'Birth' and 'will' messages of MQTT have been removed (it is better to configure them via automations to also provide the identifier of the Home Assistant's instance)
- Activation of Home Assistant's *Advanced Mode* (https://www.home-assistant.io/blog/2019/07/17/release-96/#advanced-mode)
- Deactivation of the flag 'Automatically close connection' to the server after being hidden for 5 minutes

## B. configuration.yaml
The basic information of the Home Assistance must be included in the `configuration.yaml` file (https://www.home-assistant.io/docs/configuration/basic). Please change the fields under the `homeassistant` topic accordingly and replace the existing file in your Raspberry Pi:
- name: `edge-gw-<X>` (being `X` a unique number)
- latitude: `<latitude>`
- longitude: `<longitude>`
- elevation: `<elevation>`
- etc

## C. automations.yaml
The current version of the `automations.yaml` in this repository includes the following automations, with their corresponding MQTT topics and data fields:
- **AUT_hum**: Forward humidity value to MQTT broker
  - <edge_gw_id>/esp32s2/<device_id>/hum &rarr; <timestamp_gw>,<humidity_value>
- **AUT_temp_ext**: Forward (external) temperature value to MQTT broker
  - <edge_gw_id>/esp32s2/<device_id>/temp-ext &rarr; <timestamp_gw>,<temperature_value>
- **AUT_hum_av**: Forward availability of the humidity sensor to the MQTT broker
  - <edge_gw_id>/esp32s2/<device_id>/hum/status &rarr; <timestamp_gw>,online
- **AUT_temp_ext_av**: Forward availability of the (external) temperature sensor to the MQTT broker
  - <edge_gw_id>/esp32s2/<device_id>/temp-ext/status &rarr; <timestamp_gw>,online
- **AUT_hum_unav**: Forward unavailability of the humidity sensor to the MQTT broker
  - <edge_gw_id>/esp32s2/<device_id>/hum/status &rarr; <timestamp_gw>,offline
- **AUT_temp_ext_unav**: Forward unavailability of the (external) temperature sensor to the MQTT broker
  - <edge_gw_id>/esp32s2/<device_id>/temp-ext/status &rarr; <timestamp_gw>,offline
- **AUT_HA_on**: Notify availability of the IoT Edge GW to the MQTT broker
  - <edge_gw_id>/status &rarr; <timestamp_gw>,online
  - <edge_gw_id>/attributes/location &rarr; {"latitude": <latitude_gw>, "longitude": <longitude_gw>}
- **AUT_HA_off**: Notify unavailability of the IoT Edge GW to the MQTT broker 
  - <edge_gw_id>/status &rarr; <timestamp_gw>,offline