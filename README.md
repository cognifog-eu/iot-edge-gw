# IoT Edge Gateway
This manual includes all the steps and source code correspoding to the installation of the IoT Edge Gateway module of COGNIFOG into a Raspberry Pi module.

## A. Prerequisites
The IoT Edge Gateway has been validated in the following platform:
- **HW**: Raspberry Pi 4B (4GB RAM and 32GB SD card)
- **OS**: Debian GNU/Linux 11 (bullseye)
- **SW**: Lightweight Kubernetes K3S (https://k3s.io/)

### A.1. Optional: WiFi permanent connection
- After connected to a WiFi network (normally, if the network is created by a laptop in the so called softAP process), the Raspberry Pi may disconnect after a certain period of time (normally, after less than one hour). One possible cause of this malfunctioning is the Power Management feature of its wireless interface. A potential solution to alleviate this problem is to disconnect the Power Management mode. To do it, it is necessary to type the following commands:
1. `sudo apt install wireless-tools`: To install the wireless tools
2. `sudo iwconfig wlan0`: To check the status of wlan0 interface
3. `sudo iwconfig wlan0 power off`: To disable Power Management feature in wlan0

To avoid having to execute this command after every Raspberry power on, a script in the systemd file can be made to run a script on startup. The instructions are summarized in the following steps:
1.	Create a bash script file named `StartScript.sh`: 
`sudo nano StartScript.sh`

2.	Write the following content inside:
```
#!/bin/bash
sudo iwconfig wlan0 power off
```

3.	Make sure the script file is executable, using the following command: 
`sudo chmod a+x StartScript.sh`
	
4.	Create a new startup Systemd File within the `/etc/systemd/system` system file having the `.service` extension. For instance, `/etc/systemd/system/ScriptService.service` file is created using the following command in the nano editor:
`sudo nano /etc/systemd/system/ScriptService.service`

5.	Write the below content to run the bash script `StartScript.sh` on startup:
```
[Unit]
Description=Custom Startup Script
 
[Service]
ExecStart=/home/ubuntu/StartScript.sh  # USE HERE YOUR OWN FOLDER
 
[Install]
WantedBy=default.target
```

- **Unit**: It stores the metadata and other information you want to store related to the script.
- **Service**: Tells the system to execute the desired service, which will run on startup.
- **Install**: Allows the service to run the WantedBy directory at the startup to handle the dependencies.

6.	Set the user executable file permissions the Service unit file using the following command:
`sudo chmod 644 /etc/systemd/system/ScriptService.service`

7.	Use the below-stated command to enable the customized service (which allows the execution on each startup):
`sudo systemctl enable ScriptService.service`

From now on, every time the system starts/reboots, the script file will execute automatically.

(Further information of the **A.1. Optional: WiFi permanent connection** step can be found in **Method 1** section of the following link: https://itslinuxfoss.com/run-script-startup-ubuntu/)

## B. K3S installation
The first thing to do is to update and upgrade your Raspberry:
1. `sudo apt update`
2. `sudo apt upgrade`

Then, in file `/boot/cmdline.txt` add `cgroup_enable=cpuset cgroup_enable=memory cgroup_memory=1` to the end of the existing line (do not add it as a new line!!) and reboot the Raspberry Pi:
`sudo nano /boot/cmdline.txt`

The next step installs K3S:
`curl -sfL https://get.k3s.io | sh -s - --write-kubeconfig-mode 644`

And that’s it! You have a Kubernetes cluster running! You can check it with the command:
`kubectl get pods -A`

(A comprehensive description of **B. K3S installation** step can be found at https://www.padok.fr/en/blog/raspberry-kubernetes. In addition, here is the official rancher documentation to install k3s: https://docs.k3s.io/quick-start)

## C. Installation of the IoT Edge Gateway manifest files
The current version of the IoT Edge Gateway consists of a set of applications split into different files (YAML manifests) to be deployed by using K3S:
- **Home Assistant**: Open-source home automation platform that allows users to control and manage various smart devices and services (https://www.home-assistant.io/)
  - `home-assistant-deploy.yaml`: Home Assistant deployment file
  - `home-assistant-service.yaml`: Home Assistant service file
- **MATTER controller add-on for Home Assistant**: Plug-in for Home Assistant to control MATTER-based end devices running over WiFi (https://www.home-assistant.io/integrations/matter/)
  - `matter-server-deploy.yaml`: Matter Server deployment file
  - `matter-server-service.yaml`: Matter Server service file
- **(Optional) Portainer**: Open-source management and user interface for Docker/Kubernetes (https://www.portainer.io/)
  - `portainer-deploy-full.yaml`: Portainer deployment file (all included)

To install all applications, copy all files into a folder of the Raspberry Pi and run the following K3S command: `kubectl apply -f .` Alternatively, each application file can be deployed by using the following K3S command: `kubectl apply -f [file].yaml`

Check that all services are properly deployed by running:
`kubectl get pods -A`

(Note that the installation of **Home Assistant** and the **Matter controller add-on** can be very slow due to the size of both images: 1.7 GB and 394 MB, respecively, at 02/01/2024)

## D. Operation
After a successful installation, Home Assistant should be running at `http://[IP_ADDRESS]:8123` 

It may be needed to configure the Matter Server add-on inside Home Assistant. To do it, follow these steps:
1. Go to Settings -> Devices & services
2. Click Add Integration
3. Type 'Matter' into the field 'Search for a brand name'
4. Click 'Matter (BETA)'
5. Check that the URL* field contains `ws://localhost:5580/ws`

## E. Other
Optionally, end devices can be configured to interact with the IoT Edge Gateway by means of the MATTER protocol. More specifically, this communication has been validated by using ESP32-S2 DevKitM-1 devices (https://docs.espressif.com/projects/esp-idf/en/latest/esp32s2/hw-reference/esp32s2/user-guide-devkitm-1-v1.html) and the firmware provided by Tasmota (https://tasmota.github.io/docs/).

A Tasmota pre-configured file with the configuration of Tasmota 13.1.0 for an ESP32-S2 DevKitM-1 device is provided in the `/tasmota` folder of the current repository. To flash an ESP32-S2 device with this firmware, visit https://tasmota.github.io/install/.