# DNS Data Exfiltration

DNS Data Exfiltration allows you to copy and transfer data from the compromised machine to your own server or device by sending the chunked version of the data through subdomains. To be able to use this technique, you would need to have outbound port 53 (DNS) open on the compromised machine. There are a bunch of DNS exfiltration tools you can use. The following steps should be taken to achieve DNS Exfil:

1. Check outbound firewall settings for port an open port 53 on the compromised machine.
2. Set the DNS server IP address to the attacker's machine.
3. Install an DNS exfiltration tool on the compromised machine.
4. Install a reciever tool on the attacker machine (usually the same tool as the victims).
5. Run wireshark on the attacker machine.
6. Transmit the file via DNS from the compromised machine to the attacker machine.
7. Save the wireshark traffic and decode the PCAP file to get the file.

## Changing the DNS Server IP address

### Windows 10 (Access to GUI)

1. Search for "Network".
2. Click on "Network and Sharing Center".
3. Click on "Change adapter settings".
4. Right click on the interface you want to change and click on "Properties".
5. Select "Internet Protocol Version 4".
6. Click on "Properties".
7. Change the "Preferred DNS server" field value.