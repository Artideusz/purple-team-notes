# Snort - An free and open-source IPS/IDS System

Snort is a multi-purpose cross-platform tool providing the following features:

- Packet sniffing.
- Logging traffic.
- A full-blown Network-Based Intrusion Detection / Prevention System.

Snort is rule-based, meaning that the packet matching mechanism is similar to those found in firewalls. It is also the one of the most popular IDS/IPS systems in 2022.

## Snort modes

Snort can be switched between 3 different modes.

### Sniffer Mode

Sniffer mode makes Snort sniff packets and output them in the console. You can enable this feature by using the following commands on Linux:

- `snort -v` - Sniff and display TCP/IP header information.
- `snort -vd` - Sniff and display TCP/IP header information AND data payload content.
- `snort -vde` - Same as above but you can also get link-layer data (MAC addresses).
- `snort -X` - Get a full packet dump (all information in a hexdump format) when sniffing.

### Packet Logger Mode

Packet Logger Mode is added by providing the `-l <DIRECTORY>` flag. You can use the same flags as in sniffer mode and send the output to a file, or to a file structure if you provide the `-K ASCII` flag, which looks something like this:

![Image of an ls command showcasing the "-K ASCII" flag.](./img/k_ascii.jpeg)

*Bare in mind that logs written in binary mode can be read by snort, but in ASCII mode, snort cannot read it.* The binary mode also enables software like `tcpdump` and `wireshark` to read the output file that snort generated.

To read a snort log, you need to use the `-r <snort_log>` flag. **You can also filter the packets using the BPF syntax** which is the same as in wireshark.

### IDS/IPS Mode

IDS mode is activated with the following flags:
- `-c <snort_config>` - A custom snort config with a list of rules to match against the packets.
- `-N` - Disable logging (This will not save anything, only show output).
- `-D` - Run snort in the background.
- `-A` - Alerting mode (this enables IDS mode) has the following options:
    - `console` - Alert on the console.
    - `none` - No alerting.

## Testing configurations

Before using snort, it is important to test your config file using the following command: `snort -c <config_path> -T`, where `-c` means we provide a custom config file, and `-T` means that we want to run snort in **test mode**.