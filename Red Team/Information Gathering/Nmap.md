# Nmap (Network mapper)

Nmap is the most popular network information gathering tool. It can:

- Check which hosts are alive on the network.
- Scan open ports on a host.
- Identify services on the host machine.
- Guess the OS of the host.
- Check if a host is vulnerable to an exploit thanks to NSE Scripts.
- And much more.

It is very useful for penetration testers, as well as system administrators.

## Useful flags and examples

Here are a couple examples of using nmap:

### Scan ports on host

This will scan top 1000 ports (default) on a machine:

```
$ nmap 10.0.0.100
```

### Show verbose information when scanning

```
$ nmap -v 10.0.0.100
```

You can also replace `-v` with `-vv` to get even more verbose information

### Scan only selected ports 

```
$ nmap -p80,443,8080,9999 10.0.0.100
```

### Scan range of ports

```
$ nmap -p1-1000 10.0.0.100
```

This command will scan all ports from 1 to 1000 on host 10.0.0.100.

### Scan all TCP ports

```
$ nmap -p- 10.0.0.100
```

The SYN Scan is set by default, so UDP will not be scanned.

### Scan UDP ports

```
$ nmap -sU 10.0.0.100
```

### My most used command

```
$ nmap -vv -sV -sC -oA nmap/nmap -p- <ip>
```
