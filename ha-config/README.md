# Home Assistant configuration templates
This section includes a set of template files to preconfigure Home Assistant. 
To apply any configuration, copy the .zip file into the Raspberry Pi, unzip it, and paste it into `/home/cognifog/iot-edge-gw/ha-config`, replacing all previously existing files.

## A. ha-config-v1.0.rar
Minimal configuration of Home Assistant, with the following configured capabilities:
- Integration with the MATTER controller add-on for Home Assistant
- Configuration of an MQTT client in Home Assistant that can be connected to an external MQTT broker with the following features:
  - MQTT broker IP address: `84.88.36.141`
  - MQTT broker port: `1883`
  - MQTT client username: `i2cat`
  - MQTT client password: `<protected>`