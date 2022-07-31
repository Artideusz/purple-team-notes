# IDS / IPS

These 2 systems save us from DDoS attacks and mass hacking.

## Intrusion Detection System (IDS)

An IDS system is software that monitors network traffic and compares the packets to rules defined inside the software configuration. They are usually installed on a device that acts as a firewall or a router. There are 2 types of IDSs:

### Host-Based (HIDS)

Host-Based Intrusion Detection Systems detect malicious traffic and system calls on the host machine, where the software would be installed. The following software are HIDSs:

- OSSEC
- Samhain

### Network-Based (NIDS)

Network-Based Intrusion Detection Systems inspect malicious traffic on the whole network. These kind of IDSs are usually installed on a router or on a device that intercepts network traffic. The following software are NIDSs:

- Snort
- Suricata