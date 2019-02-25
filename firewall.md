---

copyright:
  years: 2017
lastupdated: "2018-11-10"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Manage Your IBM Firewalls
{: #manage-your-ibm-firewalls}

The Virtual Router Appliance (VRA) has the ability to process firewall rules to protect the VLANs routed through the device. The firewalls in the VRA can be broken into two steps:

1. Defining one or more sets of rules.
2. Applying a set of rules to an interface or a zone. A zone consists of one or more network interfaces.

It is important to test firewall rules after creation to ensure the rules work as intended and that new rules do not restrict administrative access to the device.

While manipulating rules on the `dp0bond1` interface, it is advised to connect to the device using `dp0bond0`. Connecting to the console using the Intelligent Platform Management Interface (IPMI) is also an option.

## Stateless vs Stateful
By default, the firewall is stateless, but it can be configured as stateful if needed. A stateless firewall will need rules for traffic in both directions, while stateful firewalls track connections and automatically allow the returning traffic of accepted flows. To configure a stateful firewall, you must dictate which rules you want to operate statefully.

To enable 'stateful' tracking of `tcp`, `udp`, or `icmp` traffic, run the following commands:

```
set security firewall global-state-policy icmp
set security firewall global-state-policy tcp
set security firewall global-state-policy udp
```

Note that the `global-state-policy` commands will only track the state of traffic that has matched a firewall rule which explicitly sets the corresponding protocol. For example:

```
set security firewall name GLOBAL_STATELESS rule 1 action accept
```

As `GLOBAL_STATELESS` does not specify `protocol tcp`, the `global-state-policy tcp` command would not apply on this rule. 

```
set security firewall name GLOBAL_STATEFUL_TCP rule 1 action accept
set security firewall name GLOBAL_STATEFUL_TCP rule 1 protocol tcp
```

In this case `protocol tcp` is explicitly defined. The `global-state-policy tcp` command would enable stateful tracking of traffic that matches rule 1 of `GLOBAL_STATEFUL_TCP`


To make individual firewall rules 'stateful':

```
set security firewall name TEST rule 1 allow
set security firewall name TEST rule 1 state enable
```
This would enable stateful tracking of all traffic that can be tracked statefully and matches rule 1 of `TEST`, regardless of the existence of `global-state-policy` commands. 

## ALG for assisted stateful tracking
A few protocols such as FTP utilise more complex sessions that the normal stateful firewall operation can track. 
There are preconfigured modules that enable these protocols to be stetfully managed.
It is suggested to disable these ALG modules, unless they are required for the successful use of the respective protocols.

```
set system alg ftp 'disable'
set system alg icmp 'disable'
set system alg pptp 'disable'
set system alg rpc 'disable'
set system alg rsh 'disable'
set system alg sip 'disable'
set system alg tftp 'disable'
```

## Firewall Rule Sets
Firewall rules are grouped together into named sets to make applying rules to multiple interfaces easier. Each rule set has a default action associated with it. Consider the following example:
```
set security firewall name ALLOW_LEGACY default-action accept
set security firewall name ALLOW_LEGACY rule 1 action drop
set security firewall name ALLOW_LEGACY rule 1 source address network-group1
set security firewall name ALLOW_LEGACY rule 2 action drop
set security firewall name ALLOW_LEGACY rule 2 destination port 23
set security firewall name ALLOW_LEGACY rule 2 log
set security firewall name ALLOW_LEGACY rule 2 protocol tcp
set security firewall name ALLOW_LEGACY rule 2 source address network-group2
```

In the ruleset, `ALLOW_LEGACY`, there are two rules defined. The first rule drops any traffic sourced from an address group named `network-group1`. The second rule discards and logs any traffic destined for the telnet port (`tcp/23`) from the address group named `network-group2`. The default-action indicates that anything else is accepted.

## Allowing Data Center Access
IBM© offers several IP subnets to provide services and support to systems running within the data center. For example, DNS resolver services are running on `10.0.80.11` and `10.0.80.12`. Other subnets are used during provisioning and support. You can find the IP ranges used in the data centers in [this topic](/docs/infrastructure/hardware-firewall-dedicated?topic=hardware-firewall-dedicated-ibm-cloud-ip-ranges).

You can allow data center access by placing the proper `SERVICE-ALLOW` rules at the beginning of the firewall rule sets with an action of `accept`. Where the rule set must be applied depends on the routing and firewall design being implemented.

It is recommended that you place the firewall rules in the location which causes the least duplication of work. For example, allowing backend subnets inbound on `dp0bond0` would be less work than allowing backend subnets outbound toward each VLAN virtual interface.

### Per-interface Firewall Rules
One method for configuring the firewall on a VRA is to apply firewall rule sets to each interface. In this case an interface can be a dataplane interface (`dp0s0`) or a virtual interface (`dp0bond0.303`). Each interface has three possible firewall assignments:

`in` - The firewall is checked against packets entering through the interface. These packets can be traversing or be destined for the VRA.

`out` - The firewall is checked against packets leaving through the interface. These packets can be traversing or originating on the VRA.

`local` - The firewall is checked against packets which are destined directly for the VRA.

An interface can have multiple rule sets applied in each direction. They are applied in the order of configuration. Note that it is not possible to firewall traffic originating from the VRA device using per-interface firewalls.

As an example, to assign the `ALLOW_LEGACY` rule set to the `in` option for the `bp0s1` interface, you would use the configuration command: 

`set interfaces dataplane dp0s1 firewall in ALLOW_LEGACY `

## Control Plane Policing (CPP)
Control plane policing (CPP) provides protection against attacks on the Virtual Router Appliance by allowing you to configure firewall policies that are assigned to desired interfaces and applying these policies to packets entering the VRA.

CPP is implemented when the `local` keyword is used in firewall policies that are assigned to any type of VRA interface, such as data plane interfaces or loopback. Unlike the firewall rules applied for packets traversing through the VRA, the default action of firewall rules for traffic entering or leaving the control plane is `Allow`. Users must add explicit drop rules if the default behavior is not desired.

The VRA provides a basic CPP rule set as template. You can merge it into your configuration by running: 

`vyatta@vrouter# merge /opt/vyatta/etc/cpp.conf`

After this rule set is merged, a new firewall rule set named `CPP` is added and applied to the loopback interface. It is recommend that you modify this rule set to suit your environment.

Please note that CPP rules cannot be stateful, and will only apply on ingress traffic.

## Zone Firewalling
Another firewall concept within the Virtual Router Appliance is zone based firewalls. In zone-based firewall operation an interface is assigned to a zone (only one zone per interface) and firewall rule sets are assigned to the boundaries between zones with the idea that all interfaces within a zone have the same security level and are allowed to route freely. Traffic is only scrutinized when it is passing from one zone to another. Zones drop any traffic coming into them which is not explicitly allowed.

An interface can either belong to a zone or have a per-interface firewall configuration; an interface cannot do both.

Imagine the following office scenario with three departments, each department with its own VLAN: 

* Department A - VLANs 10 and 20 (interface dp0bond1.10 and dp0bond1.20)
* Department B - VLANs 30 and 40 (interface dp0bond1.30 and dp0bond1.40)
* Department C - VLAN 50 (interface dp0bond1.50)

A zone can be created for each department and the interfaces for that department can be added to the zone. The following example illustrates this: 
```
set security zone-policy zone DEPARTMENTA interface dp0bond1.10
set security zone-policy zone DEPARTMENTA interface dp0bond1.20 
set security zone-policy zone DEPARTMENTB interface dp0bond1.30 
set security zone-policy zone DEPARTMENTB interface dp0bond1.40 
set security zone-policy zone DEPARTMENTC interface dp0bond1.50
```

The `commit` command populates each zone as an interface and the default drop rules discard any traffic trying to enter the zone from the outside. In the example, VLAN 10 and 20 can pass traffic since they are in the same zone (`DEPARTMENTA`) but VLAN 10 and VLAN 30 cannot pass traffic because VLAN 30 is in a different zone (`DEPARTMENTB`).

The interfaces within each zone can pass traffic freely and rules can be defined for interaction between the zones. A rule set is configured from the point of view of leaving one zone to another zone. 

The following command shows an example of how to configure a rule:

`set security zone-policy zone DEPARTMENTC to DEPARTMENTB firewall ALLOW_PING`

This command associates the transition from DEPARTMENTC to DEPARTMENTB with the rule set named `ALLOW_PING`. Traffic entering the DEPARTMENTB zone from the DEPARTMENTC zone would be checked against this rule set.

It is important to understand that this assignment from zone DEPARTMENTC going into zone DEPARTMENTB does not make any statement about the inverse. If there are no rules allowing traffic from zone DEPARTMENTB into zone DEPARTMENTC, then traffic (ICMP replies) will not get back to the hosts in DEPARTMENTC.

`ALLOW_PING` would be applied as an `out` firewall on the interfaces of DEPARTMENTB zone (dp0bond1.30 and dp0bond1.40). As this is installed by the zone policy, only traffic originating from the source zone's interfaces (dp0bond1.50) would be checked against the ruleset.

## Session and Packet Logging
The VRA supports two types of logging:

1. Session logging.  Use ``security firewall session-log`` command to configure firewall session logging.
  
	For UDP, ICMP, and all non-TCP flows, a session transitions to four states over the lifetime of the flow. For each transition, you can configure the VRA to log a message. TCP has a larger number of state transitions, each of which can be configured to log.  

2. Per packet logging. Include keyword ``log`` in firewall or NAT rule to log every network packet that matches the rule.

	Per-packet logging occurs in the packet forwarding paths and generates large amounts of output. It can greatly reduce the throughput of the VRA and dramatically increase the disk space used for the log files. We recommend using per packet logging only for debugging purposes. For all operational purposes, stateful session logging should be used.