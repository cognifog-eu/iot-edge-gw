# ARCA OS Embedded
To deploy the IoT Edge Gateway on the embedded version of ARCA Trusted OS, it is recommended to follow the installation guide provided by CYSEC.

## A. Configuring a Raspberry Pi as a WiFi Access Point at 2.4 GHz
**This setup requires a Raspberry Pi using ARCA OS and iwd as system network service.**

**This setup also requires that the Raspberry Pi's WiFi interface is not used for Internet access. Instead, the Raspberry Pi should be connected to the Internet via its Ethernet interface.**

**This solution does not survive a reboot and needs to be applied every time the Raspberry Pi is restarted.**

- **Step 1.** It is needed to check if *iwd* is allowed to perform network configuration on interfaces it manages. This can be accomplished by adding the following setting to `/etc/iwd/main.conf`:
```
[General]
EnableNetworkConfiguration=true
```

- **Step 2.** Create `/var/lib/iwd/ap` directory if it doesn't exist yet:

`sudo mkdir -p /var/lib/iwd/ap/`

- **Step 3.** Place the following contents in the file `/var/lib/iwd/ap/<ssid>.ap` where `<ssid>` will be used as the SSID of the Access Point when it is started:
```
[Security]
Passphrase=<password>

[IPv4]
Address=192.168.6.1
Gateway=192.168.6.1
Netmask=255.255.255.0
DNSList=8.8.8.8
```

- **Step 4.** Start *iwctl* and switch your device into AP mode. Then start the AP mode with the just-created profile:
```
iwctl 
[iwd]# device wlan0 set-property Mode ap
[iwd]# ap wlan0 start-profile <ssid>
```

- **Step 5.** Allow IP forwarding and set up the NAT:
```
sudo sysctl -w net.ipv4.ip_forward=1
sudo iptables -t nat -A POSTROUTING -s 192.168.6.0/24 -o end0 -j MASQUERADE
sudo iptables -A FORWARD -i wlan0 -j ACCEPT
```

Matter uses IPv6 for its operational communications, and leverages both IPv6 Unicast and Multicast addressing. Some commands are required to configure IPv6 in the IoT edge GW running ARCA OS Embedded, thus ensuring the proper operation of the MATTER communication protocol. 
- **Step 6.** (Optional) Ensuring MATTER operation in IPv6
```
sysctl -w net.ipv6.conf.all.disable_ipv6=1
sysctl -w net.ipv6.conf.wlan0.disable_ipv6=0
sysctl -w net.ipv6.conf.end0.disable_ipv6=0

ip -6 route add default dev end0
ip -6 route add fe80::/64 dev wlan0 metric 1
```
(Note that *end0* refers to the Ethernet interface and *wlan0* refers to the WiFi interface of the IoT edge GW, respectively)

(Further information can be found in https://iwd.wiki.kernel.org/ap_mode)

## B. Solving issues with Home Assistant (temporary method)
**This solution does not survive a reboot and needs to be applied every time the Raspberry Pi is restarted.**

If experiencing issues accessing the **Home Assistant** interface on port 8123, it may be due to firewall restrictions. Therefore, it is recommended to follow these steps to temporarily disable the firewall. Please note that disabling the firewall should be done cautiously and is intended for troubleshooting purposes.

- **Step 1. Set Default Policies to ACCEPT:** Setting the default policies to ACCEPT ensures that you won't be locked out of your server via SSH.
```
sudo iptables -P INPUT ACCEPT
sudo iptables -P FORWARD ACCEPT
sudo iptables -P OUTPUT ACCEPT
```

- **Step 2. Flush nat and mangle Tables, Flush All Chains, and Delete Non-Default Chains:** This step flushes the nat and mangle tables, clears all chains, and deletes non-default chains.
```
sudo iptables -t nat -F
sudo iptables -t mangle -F
sudo iptables -F
sudo iptables -X
```

- **Step 3. Additional Cleanup (if needed):** If the previous steps don't work, this additional command can help in flushing any remaining rules.
```
sudo iptables -F
```

By following these steps, you are essentially allowing all traffic temporarily, which might expose your server to potential security risks. After resolving the issue with Home Assistant, it's recommended to re-enable the firewall and configure it properly.

**Be careful: the modification of *iptables* can impact other services, like the configuration of the Raspberry Pi as an Access Point.**

For further details and explanation, you can refer to the following link: https://www.digitalocean.com/community/tutorials/how-to-list-and-delete-iptables-firewall-rules#flushing-all-rules-deleting-all-chains-and-accepting-all 

## C. Solving issues with Home Assistant (permanent method)
If experiencing issues accessing the **Home Assistant** interface on port 8123, it may be due to firewall restrictions. The commands clear all active rules from the iptables (IPv4) and ip6tables (IPv6) firewalls permanently, effectively disabling any configured network traffic filtering.
```
echo "" > /etc/iptables/iptables.rules
echo "" > /etc/iptables/ip6tables.rules
```