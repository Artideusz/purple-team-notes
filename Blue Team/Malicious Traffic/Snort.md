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

If you want to listen on a different interface, you can use the `-i <interface>` flag.

### Packet Logger Mode

Packet Logger Mode is added by providing the `-l <DIRECTORY>` flag. You can use the same flags as in sniffer mode and send the output to a file, or to a file structure if you provide the `-K ASCII` flag, which looks something like this:

![Image of an ls command showcasing the "-K ASCII" flag.](./img/k_ascii.jpeg)

*Bare in mind that logs written in binary mode can be read by snort, but in ASCII mode, snort cannot read it.* The binary mode also enables software like `tcpdump` and `wireshark` to read the output file that snort generated.

To read a snort log, you need to use the `-r <snort_log>` flag. **You can also filter the packets using the BPF syntax** which is the same as in wireshark. To do this, you just use a BPF rule at the end of the command: `snort -r snort.log.1234 'icmp and port 8080'` ([here](https://www.gigamon.com/content/dam/resource-library/english/guide---cookbook/gu-bpf-reference-guide-gigamon-insight.pdf) is a cool cheatsheet for BPF).

*IMPORTANT*, if you do not provide a custom log location using the `-l` flag, snort will default to `/var/log/snort/` to save the snort AND alert files (if a config file is provided).

### IDS Mode

IDS mode is activated with the following flags:
- `-c <snort_config | rules_file>` - A custom snort config with a list of rules to match against the packets.
- `-N` - Disable logging (This will not create a snort.log file, only show output).
- `-D` - Run snort in the background.
- `-A` - Alerting mode (this enables IDS mode) has the following options:
    - `console` - A simple alert on the console. (console)
    - `cmg` - An alert with header type and payload in hex and text. (console)
    - `full` - Show all possible information about the alert (default). (alert file)
    - `fast` - Shows timestamp, source and dest IP and ports. (alert file)
    - `none` - No alerting.
- `-l <log_directory>` - Enable logging. (needed for the alert and snort file)

*For `full` and `fast` alert options, the `-l` is mandatory in order to save the `alert` file.*

### IPS mode

IPS mode is the same as the IDS mode, but you also allow snort to drop packets when they hit an alert rule.

### Bonus mode: PCAP Investigation Mode

This mode is very useful for when you have a PCAP file to analyze. You can use the IDS features on the PCAP file to get suspicious packets in an alert without even manually analyzing the file yourself. To do this, you have to use the `-r <pcap>` flag **or `--pcap-list="<pcap1_file> <pcap2_file> ..."`** along with the `-c <rules>` flag to alert suspicious packets that exist within the files.

This is a simple example: `snort -q -c /etc/snort/custom/malicious.rules --pcap-list="1.pcap 2.pcap" -A full -l .`. The command flag do the following:

- `-q` - Quiet mode (no output).
- `-c` - Provide a snort config file or rules file.
- `--pcap-list` - Provide multiple PCAP files to analyze (To provide one file, you can use `-r` or `--pcap-single=`).
- `-A` - Alert mode (needed to activate alerts).
- `-l` - Provide directory for logs.

If you want to analyze in the console, you can also use `-A console` with the `--pcap-show` flag to prevent alerts from being merged together and making it harder to analyze.

## Configurations

Before using snort, it is important to test your config file using the following command: `snort -c <config_file | rules_file> -T`, where `-c` means we provide a custom config file, and `-T` means that we want to run snort in **test mode** (which means that snort will just check if the file is valid and close). 

If a config file contains a rule with type `alert`, snort will generate an alert file (if the packet is successfully matched against that rule AND is enabled by using the `-A` flag) which will be in `/var/log/snort/alert` if we do not provide the `-l <log_path>` flag.

### Creating rules!

The rules structure is the following - `<action> <procotol> <source_IP> <source_PORT> <direction> <dest_IP> <dest_PORT> (<opt_k1>:<opt_v1>;<opt_k2>:<opt_v2>;...;)`. Here is the explanation of each field:

- `<action>` - The action type:
    - `alert` - Trigger an alert.
    - `drop` - Drop the packet (no response).
    - `reject` - Reject the packet (RST response).
    - `sdrop` - Drop the packet but not log it.
- `<protocol>` - The protocol type:
    - `tcp`
    - `udp`
    - `icmp`
- `<direction>` - The direction in which the packet is going (*Note that `<-` does not exist*):
    - `<>` - Both directions are checked.
    - `->` - From src to dest.
- `<opt_kX>:<opt_vX>` - Key-Value rule options:
    - `msg:"<msg_string>"` - The `msg_string` that should be outputted when alerting.
    - `reference:<id_system>, <id>` - References to external attack id systems (like CVE or bugtraq). `id_system` must be the id system, like `cve` (here is a [list](http://manual-snort-org.s3-website-us-east-1.amazonaws.com/node31.html#SECTION00442000000000000000) made by snort) and the `id` is the id of the related vulnerability, like `CVE-2021-44228`.
    - `sid:<snort_id>` - The unique id of the snort rule (for plugins and other programs). It is recommended that the snort rule is `>=1_000_000`, since lower ids are reserved for snort.
    - `rev:<revision_number>` - The revision (or version) of the snort rule. You usually set the `revision_number` to 1, and increment when you create an improved version of the rule.
    - `classtype:<class_name>` - The class type of the rule to help categorize the type of attack or unusuall traffic. You can find the table of class types [here](http://manual-snort-org.s3-website-us-east-1.amazonaws.com/node31.html#SECTION00446200000000000000).
    - `priority:<priority_number>` - The severity of the alert. The `priority_number` is set by the `classtype` option, but if you have to overwrite it, you can use this option. You can also use this option without the `classtype` being set to define a severity level for the rule. There is no predefined range for the priority level.
    - `content:"<text value>"` | `content:"|<hex value (" " seperator)>|"` - The content in the packet payload, arguably the most important option that snort offers.
    - `protected_content:"<MD5|SHA256|SHA512 value>, length:<payload_len>"` - Same as `content`, but string is hashed so it is protected (`length` option is also needed in order for the option to work, which defines the length of the original payload length BEFORE it was hashed). This is very useful if the rules are accessible by the outside world, but also is computationally expensive.
    - Many other options are available [here](http://manual-snort-org.s3-website-us-east-1.amazonaws.com/node32.html).
    
### Note to self

- Check if `--daq afpacket -i eth0:eth1` enables inline mode (if `-Q` is even needed).
- How can one send alert information to a remote server (for example, a splunk server on another machine).

