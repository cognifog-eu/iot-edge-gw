# Managing STAs associated to the hotspot

The following section provides a set of commands for the IoT Edge GW to manage the associated STAs when it is acting as a WiFi Hotspot. There are common and specific commands for **nmcli** (Debian) and **iwctl** (ARCA OS). In several of these commands, `wlan0` refers to the name of the WiFi interface used by the IoT Edge GW in hotspot mode.

(For more advanced tools to list the connected devices on a WiFi Access Point, check https://www.baeldung.com/linux/list-devices-wireless-access-point)

## A. Common commands for both nmcli and iwctl

Regardless of using **nmcli** in Debian or **iwctl** in ARCA OS, there are a bunch of useful commands to get more info on the network and connected devices:
- `iw dev`: Lists all Wi-Fi interfaces and their details, like the interface name, type (managed, AP, etc.), and current SSID if connected.
- `iw dev wlan0 station dump`: Shows connected devices to the WiFi interface, including MAC addresses, signal strength, and data rate.
- `sudo arp | sort | grep wlan0`: Displays and sorts the ARP table, showing the IP addresses, MAC addresses, and network interfaces of devices associated with the `wlan0` interface.
- `ip neigh show` and `ip neigh show | grep wlan0`: Displays the Neighbor Table listing the IP addresses, MAC addresses, and status of devices.

## B. Specific commands for nmcli (Debian)

The following commands are only valid when using **nmcli** in Debian:
- `nmcli con s`: Displays the current connections of the host.
- `nmcli con s Hotspot`: Displays a set of parameters of the hotspot.

## C. Specific commands for iwctl (ARCA OS)

The following commands are only valid when using **iwctl** in ARCA OS:
- `iwctl ap wlan0 show`: Displays some information on the hotspot and its connected devices.