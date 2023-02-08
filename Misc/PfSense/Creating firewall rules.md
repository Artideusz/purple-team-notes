# Creating firewall rules - pfSense

The best practices when creating firewall rules are:

1. Deny all traffic first.
2. Add anti-spoofing filters (Blocking private IP addresses appearing from the outside "WAN")
3. Allow protocol communication (be as specific and restrictive as possible)
4. Deny and monitor.
5. Deny and alert.

## Web GUI

Very soon!

## Resources

- https://www.trendmicro.com/vinfo/pl/security/news/security-technology/best-practices-deploying-an-effective-firewall\
- https://www.sans.org/media/score/checklists/FirewallChecklist.pdf
- https://docs.rackspace.com/support/how-to/best-practices-for-firewall-rules-configuration/