---

copyright:
  years: 2017
lastupdated: "2019-11-14"

keywords: vfp, ipsec, firewall

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank_"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}

# Configuring a VFP interface with IPsec and Zone Firewalls
{: #configuring-a-vfp-interface-with-ipsec-and-zone-firewalls}

When an IPsec datagram arrives, it is processed through the firewall rules and then de-encapsulated. The new datagram that emerges is not associated with an interface at all. Normally, this is not a problem, and can go on to the destination interface, but zone firewalls will prevent the datagram from progressing. Any datagram that does not come from an interface in a zone policy is dropped. A VFP interface, however, informs the zone firewall that the datagram did come from an interface, which allows rules to be applied.
{: shortdesc}

To configure a VFP interface to work with IPsec traffic, first create a feature point by defining the VFP with a single IP:

```
set interfaces virtual-feature-point vfp0 address '192.168.123.123/32'
```

Then add the VFP to the tunnel:

```
set security vpn ipsec site-to-site peer 50.23.177.59 tunnel 1 uses 'vfp0'
```

Next add the interface to a zone (in this case the INTERNET zone with dp0bond1):

```
set security zone-policy zone INTERNET interface 'vfp0'
```

Now ping the server:

```
[root@acs-jmat-migserver ~]# ping -c 5  172.16.100.1
PING 172.16.100.1 (172.16.100.1) 56(84) bytes of data.
64 bytes from 172.16.100.1: icmp_seq=1 ttl=63 time=44.9 ms
64 bytes from 172.16.100.1: icmp_seq=2 ttl=63 time=44.7 ms
64 bytes from 172.16.100.1: icmp_seq=3 ttl=63 time=44.6 ms
64 bytes from 172.16.100.1: icmp_seq=4 ttl=63 time=44.3 ms
64 bytes from 172.16.100.1: icmp_seq=5 ttl=63 time=44.8 ms

--- 172.16.100.1 ping statistics ---

5 packets transmitted, 5 received, 0% packet loss, time 4006ms

rtt min/avg/max/mdev = 44.377/44.728/44.996/0.243 ms
```

It is possible to either use the same VFP interface on multiple tunnels, or create different VFPs for other tunnels, just ensure that any additional VFPs are in a zone and have rules that allow the traffic for the unencapsulated frames.

It is also possible to add VFPs to their own zone. For example:

```
set security zone-policy zone SERVERS to TUNNEL firewall 'ALLOWALL'
set security zone-policy zone TUNNEL interface 'vfp0'
set security zone-policy zone TUNNEL to SERVERS firewall 'ALLOWALL'
```

While the VFP interface is a "real" interface in that it can be monitored with commands like `tshark` and can route traffic directly, it isn't useful to do so. Any traffic that arrives on the other end that does not match the policy of the tunnel will get dropped. It is not as versatile as a VTI interface would be.
