# Firewalls

## What are firewalls?

Firewalls is software that inspects incoming packets and are matched against **rules**. Rules can be set to `pass`, `block` or `reject` packets. Classical firewalls **do not** check the content of the packets; only the header information like:

- The source IP address.
- The destination IP address.
- The source and destination ports.
- The interface used.
- The transport and application protocol used.

## Types of firewalls

### Classical Firewalls

Classical Firewalls is sometimes found inside our routers for example. They inspect traffic and check the header information of every packet that goes through the router to determine if the packet should be rejected, blocked or passed depending on the rules set. Most common rule types are the following:

- A pass rule means that if the packet matches the rule, it will be passed and not blocked, allowing for communication.
- A block rule means that if the packet matches, it will be dropped and ignored, causing the sender to get no response.
- A reject rule means that if the packet matches the rule, it will be dropped, but the sender will get a RST packet - for TCP, and a ICMP port unreachable for UDP.

Rules are matched in a "waterfall" style, which means that the first rule is compared against the packet and if it doesn't match, it will be passed onto the next rule.

The classical firewall only inspect the header content of the packets, not the payload data.

### Application Firewall 

Application Layer Firewalls are software that controls the traffic flow from the 7th layer of the OSI model, which is the Application layer. It inspects protocols like HTTP/HTTPS, FTP, SSH, and much more. There are 2 types of Application Firewalls:

#### Network-Based Applciation Firewalls

This type of firewall focuses mainly on the network traffic, inspecting and matching every packet against rules defined by the NAF. This is similar to the Classical Firewall with the addition of comparing the content of the packet (e.g., HTTP POST request data, FTP USER/PASS attempts) to rules defined. There are 2 types of network-based application firewalls:

- Active, which changes the packet traffic after the packet brakes a rule.
- Passive, which does not change the packet traffic in any way, but notifies and alerts if the packet is deemed malicious by the firewall.

A Web Application Firewall (WAF) is a specialized Active Network-Based Application Firewall that is used for HTTP traffic and is used and deployed on the reverse proxy side. 

#### Host-Based Application Firewalls

This type of firewall is installed on a machine and it acts as an IPS/IDS for system processes and network communication with the host. It inspects system calls invoked by an application, preventing some viruses from making more damage on the target network. An example of a Host-Based Application Firewall is the Windows Firewall.

A better, more modern alternative is the use of **sandboxes**.

### Intrusion Detection / Prevention System (IDS/IPS)

Intrusion Detection System is software that notifies and alerts the administrator or sent to a SIEM system when malicious or unknown traffic is detected. The software also usually is an IPS at the same time, which means that it not only notifies the SIEM system about the incident, but also blocks further traffic from the malicious source. Examples of Intrusion Detection Systems include:

- Snort - An open-source Network-Based Intrusion Detection/Prevention System which is free to use. It is the most popular option.
- Suricata - Also an open-source Network-Based IDS/IPS which operates on an application layer.
- OSSEC - A free Host-Based IDS/IPS.

## Resources

- https://en.wikipedia.org/wiki/Application_firewall
- https://www.f5.com/services/resources/glossary/application-firewall
- https://www.comparitech.com/net-admin/network-intrusion-detection-tools/