---

copyright:
  years: 2017, 2025
lastupdated: "2025-06-30"

keywords:

subcollection: virtual-router-appliance

---

{{site.data.keyword.attribute-definition-list}}

# Setting up an IPsec tunnel that works with zone firewalls
{: #setting-up-an-ipsec-tunnel-that-works-with-zone-firewalls}

In previous versions of {{site.data.keyword.vra_full}}, IPsec tunnels that used policy-based routing did not work well with zone firewalls. With version 18.01, a new set of commands addresses this issue, by using "virtual feature points" to enable the traffic from designated tunnels. The feature point acts as an interface that provides an endpoint to include in a zone policy configuration.
{: shortdesc}

The following example provides the configuration of two systems with IPsec between them:

## System A
{: #system-a}

```sh
vyatta@acs-jmat-migsim01:~$ show configuration commands | grep ipsec
set security vpn ipsec esp-group ESP01 pfs 'enable'
set security vpn ipsec esp-group ESP01 proposal 1 encryption 'aes256'
set security vpn ipsec esp-group ESP01 proposal 1 hash 'sha2_512'
set security vpn ipsec ike-group IKE01 proposal 1 dh-group '2'
set security vpn ipsec ike-group IKE01 proposal 1 encryption 'aes256'
set security vpn ipsec ike-group IKE01 proposal 1 hash 'sha2_512'
set security vpn ipsec site-to-site peer 50.23.177.59 authentication pre-shared-secret '********'
set security vpn ipsec site-to-site peer 50.23.177.59 default-esp-group 'ESP01'
set security vpn ipsec site-to-site peer 50.23.177.59 ike-group 'IKE01'
set security vpn ipsec site-to-site peer 50.23.177.59 local-address '169.47.243.43'
set security vpn ipsec site-to-site peer 50.23.177.59 tunnel 1 local prefix '172.16.200.1/30'
set security vpn ipsec site-to-site peer 50.23.177.59 tunnel 1 remote prefix '172.16.100.1/30'
```

## System B
{: #system-b}

```sh
vyatta@acs-jmat-1801-1a:~$ show configuration commands | grep ipsec
set security vpn ipsec esp-group ESP01 pfs 'enable'
set security vpn ipsec esp-group ESP01 proposal 1 encryption 'aes256'
set security vpn ipsec esp-group ESP01 proposal 1 hash 'sha2_512'
set security vpn ipsec ike-group IKE01 proposal 1 dh-group '2'
set security vpn ipsec ike-group IKE01 proposal 1 encryption 'aes256'
set security vpn ipsec ike-group IKE01 proposal 1 hash 'sha2_512'
set security vpn ipsec site-to-site peer 169.47.243.43 authentication pre-shared-secret 'iamsecret'
set security vpn ipsec site-to-site peer 169.47.243.43 default-esp-group 'ESP01'
set security vpn ipsec site-to-site peer 169.47.243.43 ike-group 'IKE01'
set security vpn ipsec site-to-site peer 169.47.243.43 local-address '50.23.177.59'
set security vpn ipsec site-to-site peer 169.47.243.43 tunnel 1 local prefix '172.16.100.1/30'
set security vpn ipsec site-to-site peer 169.47.243.43 tunnel 1 remote prefix '172.16.200.1/30'
```

This configuration sets up a generic tunnel that routes `172.16.x.x` traffic between the two systems. System B has `172.16.100.1` as a loopback address to provide an endpoint to test with, while System A has a virtual machine on a routed VLAN to provide source traffic across the tunnel.

You can see the results here:

```sh
[root@acs-jmat-migserver ~]# ping -c 5 172.16.100.1
PING 172.16.100.1 (172.16.100.1) 56(84) bytes of data.
64 bytes from 172.16.100.1: icmp_seq=1 ttl=63 time=44.5 ms
64 bytes from 172.16.100.1: icmp_seq=2 ttl=63 time=44.6 ms
64 bytes from 172.16.100.1: icmp_seq=3 ttl=63 time=44.8 ms
64 bytes from 172.16.100.1: icmp_seq=4 ttl=63 time=44.9 ms
64 bytes from 172.16.100.1: icmp_seq=5 ttl=63 time=44.6 ms
--- 172.16.100.1 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4006ms
rtt min/avg/max/mdev = 44.578/44.750/44.993/0.244 ms
```
{: screen}

This example illustrates bidirectional traffic across this IPsec tunnel. Next, you can apply a simple "allow all" zone policy to all interfaces on System A:

```sh
set security firewall name ALLOWALL default-action 'drop'
set security firewall name ALLOWALL rule 10 action 'accept'
set security firewall name ALLOWALL rule 10 protocol 'tcp'
set security firewall name ALLOWALL rule 10 state 'enable'
set security firewall name ALLOWALL rule 20 action 'accept'
set security firewall name ALLOWALL rule 20 protocol 'icmp'
set security firewall name ALLOWALL rule 20 state 'enable'
set security firewall name ALLOWALL rule 30 action 'accept'
set security firewall name ALLOWALL rule 30 protocol 'udp'
set security firewall name ALLOWALL rule 30 state 'enable'
```

Then, add policies between all three interfaces:

```sh
set security zone-policy zone INTERNET interface 'dp0bond1'
set security zone-policy zone INTERNET to PRIVATE firewall 'ALLOWALL'
set security zone-policy zone INTERNET to SERVERS firewall 'ALLOWALL'
set security zone-policy zone PRIVATE interface 'dp0bond0'
set security zone-policy zone PRIVATE to INTERNET firewall 'ALLOWALL'
set security zone-policy zone PRIVATE to SERVERS firewall 'ALLOWALL'
set security zone-policy zone SERVERS interface 'dp0bond0.1341'
set security zone-policy zone SERVERS to INTERNET firewall 'ALLOWALL'
set security zone-policy zone SERVERS to PRIVATE firewall 'ALLOWALL'
```

After the policies are applied, traffic no longer flows, despite being set to `allow all`.

## Policy-based configuration with different peers
{: #peer-based-config-different-peer}

This section outlines the configuration of IPsec VPN tunnels in a policy-based setup, which involves multiple remote peers.

Peer 1 has an IP address of `10.10.10.1`

```sh
set security vpn ipsec site-to-site peer 10.10.10.1 authentication pre-shared-secret '********'
set security vpn ipsec site-to-site peer 10.10.10.1 default-esp-group ESP01
set security vpn ipsec site-to-site peer 10.10.10.1 ike-group IKE01
set security vpn ipsec site-to-site peer 10.10.10.1 local-address 10.10.9.1

```

This configuration sets up Tunnel 2 to the Peer 1 at `10.10.10.1`

```sh
set security vpn ipsec site-to-site peer 10.10.10.1 tunnel 2 local prefix 192.168.3.1/32
set security vpn ipsec site-to-site peer 10.10.10.1 tunnel 2 remote prefix 192.168.4.1/32
set security vpn ipsec site-to-site peer 10.10.10.1 tunnel 2 uses vfp2

```

Peer 2 has an IP address of `192.168.1.1`

```sh
set security vpn ipsec site-to-site peer 192.168.1.1 authentication pre-shared-secret '********'
set security vpn ipsec site-to-site peer 192.168.1.1 default-esp-group ESP01
set security vpn ipsec site-to-site peer 192.168.1.1 ike-group IKE01
set security vpn ipsec site-to-site peer 192.168.1.1 local-address 10.10.9.1

```

This configuration sets up Tunnel 1 to the Peer 2 at `192.168.1.1`

```sh
set security vpn ipsec site-to-site peer 192.168.1.1 tunnel 1 local prefix 192.168.3.1/32
set security vpn ipsec site-to-site peer 192.168.1.1 tunnel 1 remote prefix 192.168.4.1/32
set security vpn ipsec site-to-site peer 192.168.1.1 tunnel 1 uses vfp1

```
When you configure policy-based IPsec VPN tunnels in Vyatta with multiple peers (for example, primary and secondary) that share a common local and remote prefix, only one tunnel remains active at a time. This behavior is expected because Vyatta can't differentiate between traffic flows for multiple tunnels that use identical prefix pairs in a policy-based setup. To enable multiple tunnels with the same prefix configuration, such as primary and secondary failover paths, use a route-based VPN. Route-based VPNs offer greater flexibility and allow concurrent operation of multiple tunnels, even when the local and remote prefixes are identical.
{: note}
