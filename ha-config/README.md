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
*To complete*

## C. automations.yaml
*To complete*

