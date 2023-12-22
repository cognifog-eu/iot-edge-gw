# IoT Edge Gateway
Source code corresponding to the IoT Edge Gateway module of COGNIFOG.

## Prerequisites
Source code has been validated in the following platform:
- **HW**: Raspberry Pi 4B - 4GB RAM
- **OS**: Debian GNU/Linux 11 (bullseye)
- **SW**: Lightweight Kubernetes K3S (https://k3s.io/)
- A K3S installation guide for Raspberry Pi can be found at https://www.padok.fr/en/blog/raspberry-kubernetes

## Files
The current version of the IoT Edge Gateway consists of 2 services split into 4 files (YAML manifests) to be deployed by using K3S:
- **Home Assistant**: Open-source home automation platform that allows users to control and manage various smart devices and services (https://www.home-assistant.io/)
  - home-assistant-deploy.yaml: Home Assistant deployment file
  - home-assistant-service.yaml: Home Assistant service file
- **MATTER controller add-on for Home Assistant**: Plug-in for Home Assistant to control MATTER-based end devices running over WiFi (https://www.home-assistant.io/integrations/matter/)
  - matter-server-deploy.yaml: Matter Server deployment file
  - matter-server-service.yaml: Matter Server service file

## Installation
Copy all 4 files into a folder of Raspberry Pi. Inside that folder, run the following command:
`kubectl apply -f .`

Check that all services are properly deployed by running:
`kubectl get pods -A`

## Operation
After a successful installation, Home Assistant should be running at http://[IP_ADDRESS]:8123 
It may be needed to configure the Matter Server add-on inside Home Assistant. To do it, follow these steps:
1. Go to Settings -> Devices & services
2. Click Add Integration
3. Type 'Matter' into the field 'Search for a brand name'
4. Click 'Matter (BETA)'
5. Check that the URL* field contains 'ws://localhost:5580/ws'