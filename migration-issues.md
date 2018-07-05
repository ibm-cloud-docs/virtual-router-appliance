---

copyright:
  years: 2017
lastupdated: "2018-05-03"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Vyatta 5400 Common Migration Issues
The following table illustrates common issues or behaior changes you may encounter after migrating from a Vyatta 5400 device to a IBM Virtual Router Appliance. In some cases, it includes workarounds to address the issues.

## Interface Based Global-State Policy for StateFul Firewall

### Issues
THe behavior when setting "State of State-policy" for stateful firewalls from release 5.1 has been changed. In versions prior to release 5.1, if you set `state - global -state -policy` of a stateful firewall, the vRouter automatically added an implicit `Allow` rule for return communication of the session Automatically.

In release 5.1 and later you must add an `Allow` rule setting on the Virtual Router Appliance. The stateful setting works for interfaces on Vyatta 5400 devices, and for protocols on VRA devices.

### Workarounds
If the `firewall-in` rule is applied on an Ingress/Inside interface then the `Firewall-out` rule must be applied on the Egress/Outside interface. Otherwise, return traffic will be dropped at the Egress/Outside interface.        

## State-Enable in Firewall Rules

### Issues
If `global-state-policy` is not configured, this behavior change is not affected.

Also, if you are using the `state enable` option in each rule instead of `global-state-policy`, the behavior change is not affected.

## Zone-based policy: Local-zone handling

### Issues
There is no "local-zone" pseudo-interface to assign to the zone-policy. 

### Workaround
This behavior can be simulated by applying a zone-based firewall to physical interfaces, and an interface-firewall to the loopback interface. The firewall in the loopback interface filters everything that ingress and egress from the router. 

For example:

```
set security zone-policy zone internal default-action 'drop'
set security zone-policy zone internal description 'Private zone'
set security zone-policy zone internal interface 'dp0bond0'
set security zone-policy zone internal to external firewall 'internal-2-external'
set security zone-policy zone internal to ovpn firewall 'internal-2-ovpn'

set security zone-policy zone ovpn default-action 'drop'
set security zone-policy zone ovpn description 'OpenVPN'
set security zone-policy zone ovpn interface 'vtun0'
set security zone-policy zone ovpn to external firewall 'ovpn-2-external'
set security zone-policy zone ovpn to internal firewall 'ovpn-2-internal'
 
set interfaces loopback lo firewall local 'Local'
 
set security firewall name ovpn-2-external default-action accept
set security firewall name ovpn-2-internal default-action accept
 
set security firewall name external-2-ovpn default-action accept
set security firewall name external-2-internal default-action accept
 
set security firewall name internal-2-external default-action accept
set security firewall name internal-2-ovpn default-action accept
 
set security firewall name Local default-action 'drop'
set security firewall name Local 'default-log'
set security firewall name Local rule 10 action 'accept'
set security firewall name Local rule 10 description 'RIP' ("/opt/vyatta/etc/cpp.conf" )
```

## Order of operation for firewalls, NAT, routing and DNS

### Issues
In a scenario where Masquerade Source NAT is deployed on an IBM Virtual Router Appliance you cannot use the firewall to determine access for hosts to the internet. This is because the post NAT address will be the same.

For Vyatta 5400 devices, this operation was possible because firewalling was done before NAT, allowing the restriction of hosts to access Internet.

### Workarounds
A new routing scheme is required for the VRA:
![routing dns](./images/routing-dns.png)

## Policy Based Routing Table

### Issues
The word "Table" in the configs is optional in v5400 Policy Based Routing but for the VRA, if the action is `accept` then the **Table** field is mandatory. If the action is `drop` on the VRA config, then the Table field is optional.

### Workarounds
"Table Main" is an available option in Vyatta 5400 Policy Base Routing. The equivalent in VRA is "routing-instance default".

## Policy Based Routing on Tunnel Interface

### Issues
On the Virtual Router Appliance PBR (Policy Based Routing) policies can be applied to data plane interfaces for inbound traffic, but not to loopback, tunnel, bridge, OpenVPN, VTI, and IP unnumbered interfaces.

### Workarounds
There are currently no workarounds for this issue.

## TCP-MSS

### Issues
IBM Virtual Router Appliance uses nftables and does not support TCP-MSS.

### Workarounds
There are currently no workarounds for this issue.

## OpenVPN

### Issues
OpenVPN does not start working when using the `push-route` parameter on the Virtual Router Appliance.

### Workarounds
Use the `openvpn-option` parameter instead of `push-route`.

## GRE/VTI over IPSEC + OSPF

### Issues
* When VIF has multiple subnets configured, traffic cannot go across those subnets in the VRA.                                             
* InterVlan Routing is not working on the VRA.

### Workarounds
Use Implicit allow rules to accept traffic across VIF interfaces.

## IPSec

### Issues
IPSec (Prefix-Based) does not work with IN Filter.

### Workarounds
Use IPSec (VTI BASED).

## IPSEC 'match-none"

### Issues
With Vyatta 5400 devices, the following firewall rule is allowed:

set firewall name allow rule 10 ipsec 

However, with IBM Virtual Router appliance, there is no IPSec.

### Workarounds
Possible alternative rules for VRA devices:

```
   match-ipsec  Inbound IPsec packets
   match-none   Inbound non-IPsec packets                                                                                                                
```

## IPSEC 'match-ipsec"

### Issues
With Vyatta 5400 devices, the following firewall rules are allowed:

set firewall name OUTSIDE_LOCAL rule 50 action 'accept'
set firewall name OUTSIDE_LOCAL rule 50 ipsec 'match-ipsec'

However, with IBM Virtual Router appliance, there is no IPSec.

### Workarounds
Add the protocols `ESP` and `AH` (IP protocols 50 and 51 respectively).

The `action` rule can be either `accept` or `drop`, as shown below:

```
set security firewall name <name> rule <rule-no> action accept
set security firewall name <name> rule <rule-no> destination port 500
set security firewall name <name> rule <rule-no> protocol udp
set security firewall name <name> rule <rule-no> action accept
set security firewall name <name> rule <rule-no> destination port 4500
set security firewall name <name> rule <rule-no> protocol udp
set security firewall name <name> rule <rule-no> action accept
set security firewall name <name> rule <rule-no> protocol ah
set security firewall name <name> rule <rule-no> action accept
set security firewall name <name> rule <rule-no> protocol esp
```

## Site-to-Site IPSEC with DNAT

### Issues
IPSec (Prefix-Based) does not work with DNAT.                                                                                                             

```
server (10.71.68.245) -- vyatta 1 (11.0.0.1) 
===S-S-IPsec=== (12.0.0.1) 
vyatta 2 -- client (10.103.0.1)
Tun50 172.16.1.245
```

The above snippet is a small setup example for DNAT translation after an IPSec packet has been decrypted in a Vyatta 5400. In the exmpale, there two vyattas, `vyatta1 (11.0.0.1)` and `vyatta2 (12.0.0.1)`. IPsec peering is established between `11.0.0.1` and `12.0.0.1`. In this case, the client is targeting `172.16.1.245` sourced from `10.103.0.1` end-to-end. The expected behavior of this scenario is that the destination address `172.16.1.245` will translate to `10.71.68.245` in the packet header.

Initially, the Vyatta 5400 device was performing DNAT on the inbound IPSec, terminating the interface and returning traffic gracefully into the IPsec tunnel using the connection tracking table. 

On a Virtual Router Appliance, the configuration does not function the same. The session is created, however the return traffic bypasses the IPsec tunnel after the conntrack table reverses the DNAT change. The VRA then sends the packet on the wire without IPsec encryption.  The upstream device is not expecting this traffic and will most likely drop it. While end to end connectivity is broken, this is intended behavior.   

### Workarounds
In order to accommodate the above networking scenario, IBM has created an RFE. 

While that RFE is currently being assessed, we recommend the following workaround:

**Interface configuration Commands**

```
set interfaces dataplane dp0p192p1 address '11.0.0.1/30'
set interfaces dataplane dp0p224p1 address '10.0.0.2/30'
set interfaces dataplane dp0p224p1 policy route pbr 'Backwards-DNAT'
set interfaces loopback lo address '169.254.1.1/24'
set interfaces tunnel tun50 address '169.254.240.1/32'
set interfaces tunnel tun50 encapsulation 'gre'
set interfaces tunnel tun50 local-ip '169.254.1.1'
set interfaces tunnel tun50 remote-ip '169.254.1.1'
```

**VPN configuration commands**

```
set security vpn ipsec esp-group ESP lifetime '30000'
set security vpn ipsec esp-group ESP proposal 1 encryption 'aes128'
set security vpn ipsec esp-group ESP proposal 1 hash 'sha1'
set security vpn ipsec ike-group IKE lifetime '60000'
set security vpn ipsec ike-group IKE proposal 1 encryption 'aes128'
set security vpn ipsec ike-group IKE proposal 1 hash 'sha1'
set security vpn ipsec site-to-site peer 12.0.0.1 authentication mode 'pre-shared-secret'
set security vpn ipsec site-to-site peer 12.0.0.1 authentication pre-shared-secret 'thekey'
set security vpn ipsec site-to-site peer 12.0.0.1 default-esp-group 'ESP'
set security vpn ipsec site-to-site peer 12.0.0.1 ike-group 'IKE'
set security vpn ipsec site-to-site peer 12.0.0.1 local-address '11.0.0.1'
set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 local prefix '172.16.1.245/30'
set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 remote prefix '10.103.0.0/24'
```

**NAT configuration commands**

```
set service nat destination rule 10 destination address '172.16.1.245'
set service nat destination rule 10 inbound-interface 'tun50'
set service nat destination rule 10 source address '10.103.0.1'
set service nat destination rule 10 translation address '10.71.68.245'
set service nat source rule 10 destination address '10.103.0.1'
set service nat source rule 10 'log'
set service nat source rule 10 outbound-interface 'tun50'
set service nat source rule 10 source address '10.71.68.245'
set service nat source rule 10 translation address '172.16.1.245'
```

**Protocols configuration commands**

```
set protocols static interface-route 172.16.1.245/32 next-hop-interface 'tun50'
set protocols static table 50 interface-route 0.0.0.0/0 next-hop-interface 'tun50'
```

**PBR configuration commands**

```
set policy route pbr Backwards-DNAT description 'Get return traffic back to tunnel for DNAT'
set policy route pbr Backwards-DNAT rule 10 action 'accept'
set policy route pbr Backwards-DNAT rule 10 address-family 'ipv4'
set policy route pbr Backwards-DNAT rule 10 destination address '10.103.0.0/24'
set policy route pbr Backwards-DNAT rule 10 source address '10.71.68.0/24'
set policy route pbr Backwards-DNAT rule 10 table '50'
```

## PPTP

### Issues
PPTP is no longer supported in the Virtual Router Appliance.                                                                                                                                                   

### Workarounds
Use the L2TP protocol instead.

## Script for IPSec Restart

### Issues
Whenever a VRRP virtual address is added to an IBM Virtual Router Appliance on a High Availability VPN, you must reinitialize the IPsec daemon. This is because the IPsec service listens only for connections to the addresses that are present on the VRA when the IKE service daemon is initialized.

For a pair of VRAs with VRRP, the standby router may not have the VRRP virtual address that is present on the device during initialization If the master router does not have that address present. Therefore, to reinitialize the IPsec daemon when a VRRP state transition occurs, run the following command on the master and backup routers:

```
interfaces dataplane interface-name vrrp vrrp-group group-id notify
```

## Recent count and Recent time

### Issues

The intent of the following rule is to limit SSH connections to 3 every 30 seconds for SSH using any address: 

```
set firewall name localGateway rule 300 action 'drop'
set firewall name localGateway rule 300 description 'Deter SSH brute force'
set firewall name localGateway rule 300 destination port '22'
set firewall name localGateway rule 300 protocol 'tcp'
set firewall name localGateway rule 300 recent count '3'
set firewall name localGateway rule 300 recent time '30'
set firewall name localGateway rule 300 state new 'enable'

```

On the IBM Virtual Router Appliance, this rule has the following issues:

* The option for recent count and recent time has been deprecated.

* Due to the previous issue, the rule cannot function as expected and will block all SSH connections to the applied interface.

### Workarounds
Use CPP instead.

## Set system conntrack issues

### Issues

```
set system conntrack expect-table-size '8192'
set system conntrack hash-size '375000'
set system conntrack modules ftp 'disable'
set system conntrack modules 'gre'
set system conntrack modules h323 'disable'
set system conntrack modules nfs 'disable'
set system conntrack modules pptp 'disable'
set system conntrack modules sip 'disable'
set system conntrack modules sqlnet 'disable'
set system conntrack modules tftp 'disable'
set system conntrack table-size '3000000'
```

## Set system conntrack timeout

### Issues
```
set system conntrack timeout icmp '30'
set system conntrack timeout other '600'
set system conntrack timeout tcp close '10'
set system conntrack timeout tcp close-wait '60'
set system conntrack timeout tcp established '432000'
set system conntrack timeout tcp fin-wait '120'
set system conntrack timeout tcp last-ack '30'
set system conntrack timeout tcp syn-recv '60'
set system conntrack timeout tcp syn-sent '120'
set system conntrack timeout tcp time-wait '60'
```

## Time based firewall

### Issues
```
set firewall name PRIV_SERVICE_IN rule 58 action 'accept'
set firewall name PRIV_SERVICE_IN rule 58 description '586427 Acesso a base de dados ate 22-2-18'
set firewall name PRIV_SERVICE_IN rule 58 destination address '10.150.156.57'
set firewall name PRIV_SERVICE_IN rule 58 destination port '3306'
set firewall name PRIV_SERVICE_IN rule 58 protocol 'tcp'
set firewall name PRIV_SERVICE_IN rule 58 source address '10.150.156.104'
set firewall name PRIV_SERVICE_IN rule 58 time startdate '2017-08-22'
set firewall name PRIV_SERVICE_IN rule 58 time stopdate '2018-02-22'
```

## TCP-MSS

### Issues
```
set interfaces tunnel tun3 address '172.17.175.45/30'
set interfaces tunnel tun3 encapsulation 'gre'
set interfaces tunnel tun3 local-ip '169.55.223.76'
set interfaces tunnel tun3 mtu '1476'
set interfaces tunnel tun3 multicast 'disable'
set interfaces tunnel tun3 policy route 'change-mss'(in 18.x unable to apply tcp-mss using PBR only option is to set on interface directly which i believe is not equivalent to pbr .
set interfaces tunnel tun3 remote-ip '104.129.200.34'
```
```
set policy route change-mss rule 1 protocol 'tcp'
set policy route change-mss rule 1 set tcp-mss '1436'
set policy route change-mss rule 1 tcp flags 'SYN
```

## Specific Application or port broken in S-S Ipsec VPN

### Issues

```
vyatta@v5600dallas09# set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 remote 
Possible Completions:
   <Enter> Execute the current command
   port    Any TCP or UDP port
   prefix  Remote IPv4 or IPv6 prefix                                                                                                                                     set security vpn ipsec esp-group ESP lifetime '30000'
set security vpn ipsec esp-group ESP proposal 1 encryption 'aes128'
set security vpn ipsec esp-group ESP proposal 1 hash 'sha1'
set security vpn ipsec ike-group IKE lifetime '60000'
set security vpn ipsec ike-group IKE proposal 1 encryption 'aes128'
set security vpn ipsec ike-group IKE proposal 1 hash 'sha1'
set security vpn ipsec site-to-site peer 12.0.0.1 authentication mode 'pre-shared-secret'
set security vpn ipsec site-to-site peer 12.0.0.1 authentication pre-shared-secret 'thekey'
set security vpn ipsec site-to-site peer 12.0.0.1 default-esp-group 'ESP'
set security vpn ipsec site-to-site peer 12.0.0.1 ike-group 'IKE'
set security vpn ipsec site-to-site peer 12.0.0.1 local-address '11.0.0.1'
set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 local prefix '172.16.1.245/30'
set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 remote prefix '10.103.0.0/24'                                          set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 remote port 21 (ftp)
```

## Significant change in logging behavior 

### Issues
There is a significant change in logging behavior between the Vyatta 5400 device and the IBM Virtual Router Appliance, from per-session to per-packet loging.

* Session logging: Records stateful session state transitions

* Packet logging: Record all packets that match the rule. Since packet logging is recorded in the log file in "packet units", there is a marked decrease in throughput and the pressure of the disk capacity.

* The logging capability of the vRouter can be used to capture firewall activity. Like any logging function, you should only enable this if you are troubleshooting a particular problem, and disable the logging as soon as you can.

The stateful firewall which manages Firewall / NAT sessions, writes in "session units". It is recommended to use session logging. Each setting example is described below:
 
**Session / logging**

* `security firewall session-log <protocol>`
* `system syslog file <filename> facility <facility> level <level>`

**Packet logging Firewall**

* `security firewall name <name> default-log <action>`
* `security firewall name <name> rule <rule-number> log`

**NAT**

* `service nat destination rule <rule-number> log`
* `service nat source rule <rule-number> log PBR`
* `policy route pbr <name> rule <rule-number> log`

**QoS**

* `policy qos name <policy-name> shaper class <class-id> match <rule-name>` 
