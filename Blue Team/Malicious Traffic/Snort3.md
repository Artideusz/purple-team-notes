# Snort 3

Snort is a multi-purpose cross-platform tool providing the following features:

- Packet sniffing.
- Logging traffic.
- And a full-blown Network-Based Intrusion Detection / Prevention System.

Snort is rule-based, meaning that the packet matching mechanism is similar to those found in firewalls. It is also the one of the most popular IDS/IPS systems in 2022.

## Snort 3 is now multithreaded

You can now use multiple threads to process packets using the `-z <thread_count>` flag.

## Preprocessors are now inspectors

- HTTP Inspection completely new
- Adds HTTP/2 support
- Fully stateful
- JIT Buffers
- New rule options
- Flow-Based for better evasion resistance
- HTTP Evader

## Snort 3 configuration

Snort 3 now uses a `lua` config file format. Snort uses the `LuaJIT` library to dynamically configure snort.

## Snort 3 rules are different

- Nets and ports are now optional.
- New buffers and other rule options.