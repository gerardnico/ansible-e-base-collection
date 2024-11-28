# Firewall


## About

There is a couple of firewall
  * ovh
  * firewalld

## OVH VPS

OVH firewall is just noob and is only placed on the interface.
Connection from ovh network will happen.

https://docs.ovh.com/gb/en/dedicated/firewall-network/

0	Authorise	TCP	all			established	Enabled
1	Authorise	TCP	143.176.206.82/32		22		Enabled
2	Authorise	TCP	all		80		Enabled
3	Authorise	TCP	all		443		Enabled
4	Authorise	TCP	all		587		Enabled
5	Authorise	TCP	all		25		Enabled
6	Authorise	TCP	143.176.206.82/32		31009		Enabled
19	Refuse	IPv4	all				Enabled

Note: 82.217.205.5/32 is landal

## Manual Configuration

  * https://datacadamia.com/os/linux/firewalld_country
  * https://datacadamia.com/os/linux/firewalld_cloudflare
  * then add to the authorized ipset, the home ip.
```bash
firewall-cmd --permanent --ipset="cloudflare-ipv4" --add-entry="143.176.206.82/32"
# Atze
firewall-cmd --permanent --ipset="cloudflare-ipv6" --add-entry="2001:1c01:4b8a:3c01:349a:6e45:f256:fe86"
```
  * then mail
```bash
firewall-cmd --permanent  --add-rich-rule='rule family="ipv4" source address="143.176.206.82/32" port port="587" protocol="tcp" accept'
```
  * then drill
```bash
firewall-cmd --permanent  --add-rich-rule='rule family="ipv4" source address="143.176.206.82/32" port port="31009" protocol="tcp" accept'
```


A list of rich rules is:
```
rule family="ipv4" source NOT ipset="nl" service name="ssh" drop
rule family="ipv4" source NOT ipset="cloudflare-ipv4" service name="http" drop
rule family="ipv6" source NOT ipset="cloudflare-ipv6" service name="http" drop
rule family="ipv6" source NOT ipset="cloudflare-ipv6" service name="https" drop
rule family="ipv4" source NOT ipset="cloudflare-ipv4" service name="https" drop
rule family="ipv4" source address="143.176.206.82/32" port port="587" protocol="tcp" accept
```
