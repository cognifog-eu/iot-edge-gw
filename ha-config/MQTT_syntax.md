# MQTT syntax
This section includes a set of template files to preconfigure Home Assistant according to the recommendations provided in the section **Migrating to a new system** from https://www.home-assistant.io/docs/configuration/.
For the COGNIFOG project, the configuration of Home Assistant in a fresh new IoT Edge Gateway is based on 3 steps:
1. To apply a new basic Home Assistant configuration, copy the `ha-config-<version>.rar` file into the Raspberry Pi, unzip it, and paste it into `/home/cognifog/iot-edge-gw/ha-config`, replacing all previously existing files.
2. Then, set the basic information of the IoT Edge Gateway according to the template provided in the file `configuration.yaml` of this repository and replace the one existing in the folder `/home/cognifog/iot-edge-gw/ha-config` of the Raspberry Pi.
3. Similarly, copy the file `automations.yaml` of this repository and replace the one existing in the folder `/home/cognifog/iot-edge-gw/ha-config` of the Raspberry Pi. This file contains all automations that enable the non-assisted operation of Home Assistant.

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

| Topic                                                       | Description             | Expected data            | Example |
|-------------------------------------------------------------|-------------------------|--------------------------|---------|
| **<edge_gw_id>/<device_type>/<device_id>/<sensor_type>**    | Sensor data publication | *_Not used_*             |         |
| <edge_gw_id>: ID of the IoT Edge GW                         |                         |                          |         |
| <edge_gw_id>/<device_type>/<device_id>/<sensor_type>/status | Sensor status           | AM2301#Temperature       |         |
| <edge_gw_id>/status                                         | Edge GW status          | AM2301#Humidity          |         |
| <edge_gw_id>/attributes/<attribute_type>                    | Edge GW attributes      |                          |         |

| **Description**         | **MQTT topic / field**                               | **Possible values** |
|-------------------------|------------------------------------------------------|---------------------|
| Sensor data publication | <edge_gw_id>/<device_type>/<device_id>/<sensor_type> |                     |
|                         | <edge_gw_id>: ID of the IoT Edge GW                  | (Integer)           |
|                         | <device_type>: Type of device                        | esp32s2             |
|                         | <device_id>: ID of the end device                    | (Integer)           |
|                         | <sensor_type>: Type of sensor                        | hum, temp-ext       |


| **MQTT method**             | **Topic**                                                                                         | **Data**                                                                |
|-----------------------------|---------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|
| **Sensor data publication** | <edge_gw_id>/<device_type>/<device_id>/<sensor_type><br>Ex: edge-gw-1/esp32s2/1/hum               | <timestamp_gw>,<sensor_value><br>Ex: 2024-01-22T18:13:30.594+01:00,45.9 |
| **Sensor status**           | <edge_gw_id>/<device_type>/<device_id>/<sensor_type>/status<br>Ex: edge-gw-1/esp32s2/1/hum/status | <timestamp_gw>,<status><br>Ex: 2024-01-22T18:15:40.067+01:00,offline    |
| **Edge GW status**          | <edge_gw_id>/status<br>Ex: edge-gw-1/status                                                       | <timestamp_gw>,<status><br>Ex: 2024-01-24T13:35:00.051+01:00,online     |
| **Edge GW attributes**      | <edge_gw_id>/attributes/<attribute_type>                                                          | {"attribute1": <attribute1_value>, "attribute2": <attribute2_value>}    |