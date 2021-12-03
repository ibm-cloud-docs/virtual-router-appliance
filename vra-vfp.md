---

copyright:
  years: 2017, 2019
lastupdated: "2019-11-14"

keywords: 

subcollection: virtual-router-appliance

---

{{site.data.keyword.attribute-definition-list}}

# Configuring a VFP interface with IPsec and zone firewalls
{: #configuring-a-vfp-interface-with-ipsec-and-zone-firewalls}

When an IPsec datagram arrives, it is processed through the firewall rules and then de-encapsulated. The new datagram that emerges is not associated with an interface at all. Normally, this is not a problem, and can go on to the destination interface, but zone firewalls prevent the datagram from progressing. Any datagram that does not come from an interface in a zone policy is dropped. However, a VFP interface, informs the zone firewall that the datagram came from an interface, which allows rules to be applied.
{: shortdesc}

To configure a VFP interface to work with IPsec traffic, first create a feature point by defining the VFP with a single IP:

```sh
set interfaces virtual-feature-point vfp0 address '192.168.123.123/32'
```

Then, add the VFP to the tunnel:

```sh
set security vpn ipsec site-to-site peer 50.23.177.59 tunnel 1 uses 'vfp0'
```

Next, add the interface to a zone (in this case the `INTERNET` zone with `dp0bond1`):

```sh
set security zone-policy zone INTERNET interface 'vfp0'
```

Now, ping the server:

```sh
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

It is possible to either use the same VFP interface on multiple tunnels, or create different VFPs for other tunnels. Make sure that any additional VFPs are in a zone and have rules that allow the traffic for the unencapsulated frames.

It is also possible to add VFPs to their own zone. For example:

```sh
set security zone-policy zone SERVERS to TUNNEL firewall 'ALLOWALL'
set security zone-policy zone TUNNEL interface 'vfp0'
set security zone-policy zone TUNNEL to SERVERS firewall 'ALLOWALL'
```

A VFP interface is not a "real" interface in the way that `dp0bond0` is (or even a VIF or TUN). It is a placeholder interface created by the firewall and NAT processes so that they can properly process traffic. You can still route traffic over a VFP like a regular interface, but `tshark` and other monitor commands reveal no traffic.
