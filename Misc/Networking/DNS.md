# DNS - Domain Name System

When opening the web browser, usually the first thing we do is type in a website address, like "youtube.com" or "google.com". Since the web browser does not know where "youtube.com" is located, it needs the IP address of that site. The Domain Name System protocol is used to translate domain names to IP addresses, so our device can know where to send the network packet. It uses name resolution resolvers to get the information needed to access a site. Let's say we want to access "google.com":

1. First, our device will check the DNS Resolver Cache to see if the IP address of the site was already found by the device.
2. If the site was never requested, the request goes to our DNS resolver (that would be the routers or ISPs IP address).
3. The resolver then sends a query to the **root name server** asking if he knows where he can find "google.com". The RNS should give the DNS resolver an IP address of a Top-Level Domain Server.
3. The DNS resolver sends information about "google.com" to the TLD server that we got from the root name server. We should get an IP address of a nameserver that knows about "google.com"
4. Then, the DNS resolver sends the request to the nameserver address we got from the TLD server to see if it has information about the domain "google.com".
5. If it has, we should get an IP address of "google.com" as a result.

So basically, every seperated value with a dot that there is in the URL will have its own nameserver:
```
https://testing.the.example.com

What the type of DNS server provides what DNS servers:

  authoritatives   authoritatives   authoritatives        TLDs
        v                v                v                v
    testing     .       the     .      example      .     com
        ^                ^                ^                ^
   authoritative         |                |                |
   authoritative----------                |                | 
   TLD  -----------------------------------                |
   root  ---------------------------------------------------
```

DNS services usually listen on the port 53.

## DNS Resolver Cache

The DNS Resolver Cache is a cache located on your computer. It is used when the site you want to access was already converted to an IP address before. The cache record consists of:

- The domain name, ex. "www.example.com"
- The IP address of that domain name.
- TTL (Time To Live).

## Root Name Servers

There are 13 root nameservers in the world:
- a.root-servers.net. - 198.41.0.4
- b.root-servers.net. - 199.9.14.201
- c.root-servers.net. - 192.33.4.12
- d.root-servers.net. - 199.7.91.13
- e.root-servers.net. - 192.203.230.10
- f.root-servers.net. - 192.5.5.241
- g.root-servers.net. - 192.112.36.4
- h.root-servers.net. - 198.97.190.53
- i.root-servers.net. - 192.36.148.17
- j.root-servers.net. - 192.58.128.30
- k.root-servers.net. - 193.0.14.129
- l.root-servers.net. - 199.7.83.42
- m.root-servers.net. - 202.12.27.33

And the IP addresses of those root nameservers are anycast IP addresses, meaning that multiple DNS servers "share" the IP address. If you were to use "f.root-servers.net" as a resolver for example, you would [first need to disable recursive mode.](https://www.quora.com/Can-I-point-DNS-directly-to-the-root-servers). The root servers respond with a list of nameservers that point to a top-level domain.

## Top-Level Domain Servers (TLD)

TLD Servers are responsible for giving you a one or a list of authoritative nameservers. Top-Level Domains have lists of millions of domains, like "google", "youtube", "example" and are provided to the DNS resolver depending on the TLD, ex. ".com", ".pro", ".mil". There are two types of TLDs:

### gTLD

General Top-Level Domain are TLDs that meant to tell the user the domain name's purpose, for example: `.com` would be for commercial purposes, `.org` for organization, `.mil` for military, and so on.

### ccTLD

Country Code TLDs are Top-Level Domains that tell the user where or in what language will the website of the domain name be. For example: `.co.uk` for sites based in the United Kingdom, `.ua` for Ukraine, and so on.

## Authoritative Nameservers

Authoritative nameservers are nameservers that contain IP addresses of specific domain and subdomains, for example `ns1.google.com` contains IP addresses for the following domains:
- `google.com`
- `images.google.com` 
- `help.google.com`
- And many many more.

## Subdomains

Subdomains are extra domains that we sometimes see before the first domain name in an URL, for example if we have the URL `a.example.com`, then `a` is a subdomain for `example`, and the TLD is `com`. There must be less or equal 63 characters in the subdomain. The whole URL must have less or equal 253 characters.

## DNS Record/Query Types

### A (IPv4)

The A record is responsible for converting a domain name into an IPv4 IP address.

### AAAA (IPv6)

The AAAA record converts a domain name into an IPv6 IP address.

### CNAME (Canonical Name)

The CNAME record converts a domain name into another domain name, so it is just an alias. If we had to get an A record of `shop.example.com` and we got a CNAME of `shops.shopify.com`, the IPv4 address of `shop.example.com` would be the A record of `shops.shopify.com`.

### MX (Mail Exchange)

The MX responsible for resolving a domain name to another domain name of an SMTP server or a mail server that manages that domain name. The MX records also have a priority value and the lowest value has the highest priority.

### TXT (Text)

The TXT record are free text fields where a DNS can store data about a domain. These are sometimes used in DNS data exfiltration. The max value for a TXT record is 255 characters.

### PTR (Pointer)

The PTR record converts an IP address into a domain name, this is called "reverse DNS lookup".

### SOA (Start Of Authority)

The SOA record is metadata about the specific DNS server.

## READ
- https://www.cloudflare.com/learning/dns/what-is-dns/
- https://en.wikipedia.org/wiki/Domain_Name_System
- https://book.hacktricks.xyz/pentesting/pentesting-dns#dns-reverse-bf
- https://www.cloudflare.com/learning/dns/dns-server-types/#recursive-resolver
- https://www.cloudflare.com/learning/dns/dns-records/dns-soa-record/
- https://en.wikipedia.org/wiki/Generic_top-level_domain
- https://ns1.com/resources/dns-types-records-servers-and-queries
- https://geekflare.com/find-subdomains/
- https://en.wikipedia.org/wiki/DNS_zone_transfer
- https://en.wikipedia.org/wiki/SOA_record
- https://en.wikipedia.org/wiki/List_of_DNS_record_types
- https://serverfault.com/questions/148401/how-do-i-check-a-ptr-record
