# ARCA OS Embedded
To deploy the IoT Edge Gateway on the embedded version of ARCA Trusted OS, it is recommended to follow the PDF document provided by CYSEC.

Additionally, if experiencing issues accessing the **Home Assistant** interface on port 8123, it may be due to firewall restrictions. Therefore, it is recommended to follow these steps to temporarily disable the firewall. Please note that disabling the firewall should be done cautiously and is intended for troubleshooting purposes.

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

For further details and explanation, you can refer to the following link: https://www.digitalocean.com/community/tutorials/how-to-list-and-delete-iptables-firewall-rules#flushing-all-rules-deleting-all-chains-and-accepting-all 