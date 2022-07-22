# DHCP Attacks
The protocol itself has a couple of problems, first is that the DHCP Servers don't really check if a device requesting an IP address is real. Second, it doesn't really authorize anything.

## What security flaws exist within this protocol?
There are 3 common types of attacks that exist within DHCP:
- **DHCP DISCOVER Flooding**
    - This attack is the little brother of DHCP Starvation, causing only DoS for devices that want to connect to the network. It works by sending a lot of DHCPDISCOVER packets per second to the DHCP Server, causing IP address exhaustion. The devices that were already connected before the attack was initialized will still be on the network, since we did not kick their IP address out using a DHCPRELEASE packet.
- **DHCP Starvation attack**
    - This attack takes advantage of the lack of authentication in the protocol. An attacker could craft DHCP packets with spoofed MAC addresses and send them to the server with an intent of exhausting and controlling the DHCP server's IP address pool. This attack is a type of DoS attack, because it causes devices that want to connect and which are connected to the network to wait until an IP address is available.
- **DHCP Spoofing**
    - The way this attack works is that a "rogue" or unauthorized DHCP server is sending fake information to DHCP clients. This attack can work as a DoS attack, or a MiTM attack, because the rogue server can also send DNS IP addresses to clients, which means that the server can eavedrop on connections between the client and server, or even replace those servers with its own. This attack usually starts right after a starvation attack.
 
## What is a DHCP Starvation Attack?
A DHCP Starvation attack is a DoS attack targeting the DHCP server. The process of this attack looks like this:
1. The attacker starts to send `DHCPDISCOVER` packets to the DHCP server, each of the packets have a random MAC address assigned for maximum damage.
2. The DHCP server sends back `DHCPOFFER` packets to non-existent devices, causing IP addresses to be non-assignable for a period of time.
3. The Program that started the DHCP starvation attack should manage the leases by sending back normal `DHCPREQUEST` messages to the server, so they don't get dropped by the DHCP server after some timeout.
New devices connecting to the network will not be able to get an IP address, causing them to not be able to communicate on the network. Old devices will be able to communicate until the lease time runs out.

Attackers usually take out the existing DHCP server to replace it with their own, so they can `MITM` attack the whole network.

## DHCP Spoofing
After DoS'ing the main DHCP server, hackers usually create their own DHCP server to intercept traffic. Usually they set the default gateway to their own IP address, so they can redirect data sent to them from local devices to the router and vice versa.

## Tools

### Yersinia 

Yersinia is a tool that exploits layer 2 vulnerabilities, such as DHCPDISCOVER attacks (short-term version of DHCP Starvation), ARP poisoning and multiple MiTM attacks. [Github Repo](https://github.com/tomac/yersinia).

RELEASE flood attack does not work for some reason on yersinia. I have not tested the DHCP rogue server that yersinia provides.

### DHCPStarv

I didn't have any luck when it comes to this tool, probably because I did not test it enough and the DHCP server I used for testing was isc-dhcp-server version 4.4.1

## Ettercap
Ettercap helps with creating a MiTM rogue DHCP server that also shows you important information (HTTP POST requests for example).