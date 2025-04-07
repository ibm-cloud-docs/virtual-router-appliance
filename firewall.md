---

copyright:
  years: 2017, 2025
lastupdated: "2025-04-07"

keywords: firewall, manage, stateless, stateful, alg, firewall, rules, CPP, Logging

subcollection: virtual-router-appliance

---

{{site.data.keyword.attribute-definition-list}}

# Managing firewalls
{: #manage-your-ibm-firewalls}

{{site.data.keyword.vra_full}} (VRA) has the ability to process firewall rules to protect the VLANs routed through the device.
{: shortdesc}

The firewalls in the VRA can be broken into two steps:

1. Defining one or more sets of rules.
2. Applying a set of rules to an interface or a zone policy. A zone consists of one or more network interfaces. A zone policy is defined for traffic traversing from one zone to another zone.

It is important to test firewall rules after creation to ensure the rules work as intended and that new rules do not restrict administrative access to the device.

While manipulating rules on the `dp0bond1` interface, it is advised to connect to the device using `dp0bond0`. Connecting to the console using the Intelligent Platform Management Interface (IPMI) is also an option.

## Stateless vs stateful
{: #stateless-vs-stateful}

By default, the firewall is stateless, but it can be configured as stateful if needed. A stateless firewall will need rules for traffic in both directions, while stateful firewalls track connections and automatically allow the returning traffic of accepted flows. To configure a stateful firewall, you must dictate which rules you want to operate statefully.

To enable 'stateful' tracking of `tcp`, `udp`, or `icmp` traffic, run the following commands:

```sh
set security firewall global-state-policy icmp
set security firewall global-state-policy tcp
set security firewall global-state-policy udp
```

Note that the `global-state-policy` commands will only track the state of traffic that has matched a firewall rule which explicitly sets the corresponding protocol. For example:

```sh
set security firewall name GLOBAL_STATELESS rule 1 action accept
```

As `GLOBAL_STATELESS` does not specify `protocol tcp`, the `global-state-policy tcp` command would not apply on this rule.

```sh
set security firewall name GLOBAL_STATEFUL_TCP rule 1 action accept
set security firewall name GLOBAL_STATEFUL_TCP rule 1 protocol tcp
```

In this case `protocol tcp` is explicitly defined. The `global-state-policy tcp` command would enable stateful tracking of traffic that matches rule 1 of `GLOBAL_STATEFUL_TCP`


To make individual firewall rules 'stateful':

```sh
set security firewall name TEST rule 1 allow
set security firewall name TEST rule 1 state enable
```

This would enable stateful tracking of all traffic that can be tracked statefully and matches rule 1 of `TEST`, regardless of the existence of `global-state-policy` commands.

## ALG for assisted stateful tracking
{: #alg-for-assisted-stateful-tracking}

A few protocols such as FTP utilize more complex sessions that the normal stateful firewall operation can track.
There are preconfigured modules that enable these protocols to be statefully managed.

It is suggested to disable these ALG modules, unless they are required for the successful use of the respective protocols.

```sh
set system alg ftp 'disable'
set system alg icmp 'disable'
set system alg pptp 'disable'
set system alg rpc 'disable'
set system alg rsh 'disable'
set system alg sip 'disable'
set system alg tftp 'disable'
```

## Firewall rule sets
{: #firewall-rule-sets}

Firewall rules are grouped together into named sets to make applying rules to multiple interfaces easier. Each rule set has a default action associated with it. Consider the following example:

```sh
set security firewall name ALLOW_LEGACY default-action accept
set security firewall name ALLOW_LEGACY rule 1 action drop
set security firewall name ALLOW_LEGACY rule 1 source address network-group1
set security firewall name ALLOW_LEGACY rule 2 action drop
set security firewall name ALLOW_LEGACY rule 2 destination port 23
set security firewall name ALLOW_LEGACY rule 2 log
set security firewall name ALLOW_LEGACY rule 2 protocol tcp
set security firewall name ALLOW_LEGACY rule 2 source address network-group2
```

In the ruleset `ALLOW_LEGACY`, there are two rules defined. The first rule drops any traffic sourced from an address group named `network-group1`. The second discards and logs any traffic destined for the telnet port (`tcp/23`) from the address group named `network-group2`. The default action indicates that anything else is accepted. A good practice is to only allow necessary traffic, then block the rest using the default action of the ruleset.

## Allowing data center access
{: #allowing-data-center-access}

IBM Cloud hosts many subnets (private service networks) that provide services and support to client servers running within its datacenters. For example, DNS resolver services run on `10.0.80.11` and `10.0.80.12`, and the default NTP server runs on `10.0.77.54`. All three are within the `10.0.64.0/29` service network. Other subnets are used during provisioning and support. You can find the private service networks used in the data centers in [this topic](/docs/infrastructure-hub?topic=infrastructure-hub-ibm-cloud-ip-ranges#service-network).

You can allow data center access by placing the proper `SERVICE-ALLOW` rules at the beginning of the firewall rule sets with an action of `accept`. Where the rule set must be applied depends on the routing and firewall design being implemented.

It is recommended that you place the firewall rules in the location which causes the least duplication of work. For example, allowing backend subnets inbound on `dp0bond0` would be less work than allowing backend subnets outbound toward each VLAN virtual interface.

### Per-interface firewall configuration
{: #per-interface-firewall-rules}

One method for configuring the firewall on a VRA is to apply firewall rule sets to each interface. In this case, an interface can be an inside VLAN interface (VIF) (such, as `dp0bond0.1303`), one of the outside interfaces (`dp0bond0` (private) or `dp0bond1` (public)), or tunnel interfaces (such as `tun0` or `vti0`). Each interface has three possible firewall assignments:

`in` - The firewall filters packets entering the interface. These packets can either travel through or be destined for the VRA.

`out` - The firewall filters packets leaving the interface. These packets can either travel through or be destined for the VRA.

`local` - The firewall is checked against packets which are destined for the VRA. The loopback interface, `lo`, can be used for filtering local inbound traffic on any interface. Firewall filters and rulesets applied to `local` are processed after any firewall rulesets that are applied as `in` to any interface.

An interface can have multiple rule sets applied in each direction. They are applied in the order of configuration. Note that it is not possible to firewall traffic originating from the VRA device using per-interface firewalls.

As an example, to assign the `ALLOW_LEGACY` rule set to the `in` option for the `dp0bond1` interface, you would use the configuration command:

`set interfaces bonding dp0bond1 firewall in ALLOW_LEGACY`

## Control Plane Policing (CPP)
{: #control-plane-policing-cpp-}

Control plane policing (CPP) provides protection against attacks on the {{site.data.keyword.vra_full}} by allowing you to configure firewall policies that are assigned to desired interfaces and applying these policies to packets entering the VRA.

CPP is implemented when the `local` keyword is used in firewall policies that are assigned to any type of VRA interface, such as data plane interfaces or loopback. Unlike the firewall rules applied for packets traversing through the VRA, the default action of firewall rules for traffic entering or leaving the control plane is `Allow`. Users must add explicit drop rules if the default behavior is not desired.

The VRA provides a basic CPP rule set as template. You can merge it into your configuration by running:

`vyatta@vrouter# merge /opt/vyatta/etc/cpp.conf`

After this rule set is merged, a new firewall rule set named `CPP` is added and applied to the loopback interface. It is recommend that you modify this rule set to suit your environment.

Please note that CPP rules cannot be stateful, and will only apply on ingress traffic.

## Zone-based firewall configuration
{: #zone-based-firewall}

In zone-based firewall configurations, one or more interfaces are assigned to a zone (although, one interface cannot be assigned to multiple zones) and firewall rule sets are applied from one zone to another. For a single zone-policy, traffic is filtered when it is passing from the first zone to the second, and the filtering only occurs on the outbound/egress interface. Zones drop any traffic coming into them which is not explicitly allowed.

An interface can either belong to a zone or have a per-interface firewall configuration; it cannot do both.

Consider the following office scenario with three departments, each department with its own VLAN:

* Department A - VLANs 10 and 20 (interfaces `dp0bond1.10` and `dp0bond1.20`)
* Department B - VLANs 30 and 40 (interfaces `dp0bond1.30` and `dp0bond1.40`)
* Department C - VLAN 50 (interface `dp0bond1.50`)

A zone can be created for each department and the interfaces for that department can be added to the zone. The following example illustrates this:

```sh
set security zone-policy zone DEPARTMENTA interface dp0bond1.10
set security zone-policy zone DEPARTMENTA interface dp0bond1.20
set security zone-policy zone DEPARTMENTB interface dp0bond1.30
set security zone-policy zone DEPARTMENTB interface dp0bond1.40
set security zone-policy zone DEPARTMENTC interface dp0bond1.50
```
{: pre}

The `commit` command populates each zone as an interface and the default drop rules discard any traffic trying to enter the zone from the outside. In the example, VLAN 10 and 20 can pass traffic since they are in the same zone (`DEPARTMENTA`) but VLAN 10 and VLAN 30 cannot pass traffic because VLAN 30 is in a different zone (`DEPARTMENTB`).

The interfaces within each zone can pass traffic freely and rules can be defined for interaction between the zones. A rule set is configured from the point of view of leaving one zone to another zone.

The following command shows an example of how to configure a rule:

`set security zone-policy zone DEPARTMENTC to DEPARTMENTB firewall ALLOW_PING`

This command associates the transition from `DEPARTMENTC` to `DEPARTMENTB` with the rule set named `ALLOW_PING`. Traffic entering the `DEPARTMENTB` zone from the `DEPARTMENTC` zone would be checked against this rule set.

It is important to understand that this assignment from zone `DEPARTMENTC` going into zone `DEPARTMENTB` does not make any statement about the inverse. If there are no rules allowing traffic from zone `DEPARTMENTB` into zone `DEPARTMENTC`, then traffic (ICMP replies) will not return to the hosts in `DEPARTMENTC`.

`ALLOW_PING` would be applied as an `out` firewall on the interfaces of the `DEPARTMENTB` zone (`dp0bond1.30` and `dp0bond1.40`). As this is installed by the zone policy, only traffic originating from the source zone's interfaces (`dp0bond1.50`) would be checked against the ruleset.

### Zone-based firewall example
{: #Zone-based-firewall-example}

The following example details a firewall environment that monitors and debugs all traffic eggressing and ingressing a VRA. 

Adding "log" to rule 1 will severly impact performance and should only be used for debugging. 
{: note}

Never leave your public interfaces wide open, as in this example.
{: tip}

```sh
set security zone-policy zone Public-and-VTI interface dp0bond1
set security zone-policy zone Public-and-VTI interface vti2
set security zone-policy zone Public-Inside interface dp0bond1.807
set security zone-policy zone Private-Outside interface dp0bond0
set security zone-policy zone Private-Inside interface dp0bond0.829

#ruleset to allow all statefully and log every packet
set security firewall name AllowAllLogALL rule 1 action accept
set security firewall name AllowAllLogALL rule 1 log
set security firewall name AllowAllLogALL rule 1 state enable

#security policy pair between public/IPSec and private network servers
set security zone-policy zone Public-and-VTI to Private-Inside firewall AllowAllLogALL
set security zone-policy zone Private-Inside to Public-and-VTI firewall AllowAllLogALL

#security policy pair between public/IPSec and public network servers
set security zone-policy zone Public-and-VTI to Public-Inside firewall AllowAllLogALL
set security zone-policy zone Public-Inside to Public-and-VTI firewall AllowAllLogALL

#security policy pair between service networks/private outside and private network customer servers
set security zone-policy zone Private-Outside to Private-Inside firewall AllowAllLogALL
set security zone-policy zone Private-Inside to Private-Outside firewall AllowAllLogALL
```
{: pre}

## Troubleshooting firewall rules
{: #Troubleshooting-firewall-rules}

You can troubleshoot both interace-based and zone-based firewall configurations.

### Troubleshooting interface-based firewall configurations
{: #Troubleshooting-interface-based-firewall-configurations}

To begin checking and troubleshooting, run these commands to understand how the policies are configured:

1. To gather which firewall rulesets are applied to each interface and in which direction, run `show configuration commands | grep firewall | grep interface`.
2. Using the output of step 1 you can find all of the rules applied to the interfaces in the flow that you are trying to check by running `show configuration commands | grep -iE '<name of firewall ruleset>'.`
3. Examine the rules to make sure that the proper subnets, ports, and protocols are allowed:
   - If you're troubleshooting service network to private server connectivity, and the ruleset is applied `in` to the VIFs (`dp0bond0.XXX`), then you must define the service networks as the destinations. This is because when traffic flows into the VIF, that is when the client server sends traffic outbound. 
   - If you're troubleshooting service network to private server connectivity, and the ruleset is applied `out` of the VIFs (`dp0bond0.XXX`), then you must define the service networks as the sources. This is because when traffic flows out of the VIF it does so towards the client servers. 
   - If you're troubleshooting service network to private server connectivity, and the ruleset is applied `in` to the `dp0bond0` interface, then you must define the service networks as the sources. This is because traffic flowing into the `dp0bond0` interface is generally destined to the servers behind the Vyatta.
   - If you're troubleshooting service network to private server connectivity, and the ruleset is applied `out` of the `dp0bond0` interface, then you must define the service networks as the destinations. This is because traffic flowing out of `dp0bond0` is in the direction away from the client servers that are behind the Vyatta. 

### Troubleshooting zone-based firewall configurations
{: #Troubleshooting-zone-based-firewall-configurations}
	
To begin checking and troubleshooting, run these commands to understand how the policies are configured:

1. To show which interfaces are in which zones, run `show configuration commands | grep zone | grep interface`.
2. Using the output from step 1, you can find the firewall ruleset applied in each zone-policy by running `show configuration commands | grep <name of zone> | grep <name of other zone>`.
3. Using the output of step 2 you can find all of the rules applied to the interfaces in the flow that you are trying to check by running `show configuration commands | grep -iE '<name of firewall ruleset>|<name of other firewall ruleset>'`.
4. Examine the rules to make sure that the proper subnets, ports, and protocols are allowed:
   - If you're troubleshooting service network to private server connectivity, and the policy is from the VIFs (`dp0bond0.XXX`) to `dp0bond0`, you must define the service networks as the destination.
   - For policies that are from `dp0bond0` to the VIFs (`dp0bond0.XXX`), you must define the service networks as the sources. 
   
The following example details the commands you can use to pull the information needed to determine if the firewall should allow the service networks.

```sh
vyatta@gateway02:~$ show configuration commands | grep zone | grep interface
set security zone-policy zone SL-Private-Servers interface 'dp0bond0.1750'
set security zone-policy zone SL-Private-Servers interface 'dp0bond0.1623'
set security zone-policy zone SL-Service interface 'dp0bond0'
 
vyatta@gateway02:~$ show configuration commands | grep SL-Private-Servers | grep SL-Service
set security zone-policy zone SL-Private-Servers to SL-Service firewall 'To-Private-Service-Network'
set security zone-policy zone SL-Service to SL-Private-Servers firewall 'To-Private-Servers'
 
vyatta@gateway02:~$ show configuration commands | grep -iE 'To-Private-Service-Network|To-Private-Servers'
set security firewall name To-Private-Servers rule 1 action 'accept'
set security firewall name To-Private-Servers rule 1 source address 'ServiceNetwork'
set security firewall name To-Private-Service-Network rule 1 action 'accept'
set security firewall name To-Private-Service-Network rule 1 destination address 'ServiceNetwork'
set security zone-policy zone SL-Private-Servers to SL-Service firewall 'To-Private-Service-Network'
set security zone-policy zone SL-Service to SL-Private-Servers firewall 'To-Private-Servers'
 
#Pull the actual subnets that I'm allowing via the defined ServiceNetwork addresses:
vyatta@gateway02:~$ show configuration commands | grep ServiceNetwork
set resources group address-group ServiceNetwork address '10.0.86.0/24'
set resources group address-group ServiceNetwork address '10.2.128.0/20'
set resources group address-group ServiceNetwork address '10.1.176.0/20'
set resources group address-group ServiceNetwork address '10.1.64.0/19'
set resources group address-group ServiceNetwork address '10.1.96.0/19'
set resources group address-group ServiceNetwork address '10.1.192.0/20'
set resources group address-group ServiceNetwork address '10.1.160.0/20'
set resources group address-group ServiceNetwork address '10.2.32.0/20'
set resources group address-group ServiceNetwork address '10.2.64.0/20'
set resources group address-group ServiceNetwork address '10.2.112.0/20'
set resources group address-group ServiceNetwork address '10.2.160.0/20'
set resources group address-group ServiceNetwork address '10.1.208.0/20'
set resources group address-group ServiceNetwork address '10.2.80.0/20'
set resources group address-group ServiceNetwork address '10.2.144.0/20'
set resources group address-group ServiceNetwork address '10.2.48.0/20'
set resources group address-group ServiceNetwork address '10.2.176.0/20'
set resources group address-group ServiceNetwork address '10.3.64.0/20'
set resources group address-group ServiceNetwork address '10.0.64.0/19'
set resources group address-group ServiceNetwork address '10.0.80.11'
set resources group address-group ServiceNetwork address '10.0.80.12'
set resources group address-group ServiceNetwork address '10.200.80.0/20'
set resources group address-group ServiceNetwork address '10.3.160.0/20'
set resources group address-group ServiceNetwork address '10.201.0.0/20'
set resources group address-group ServiceNetwork address '10.3.80.0/20'
set security firewall name To-Private-Servers rule 1 source address 'ServiceNetwork'
set security firewall name To-Private-Service-Network rule 1 destination address 'ServiceNetwork'
```
{: pre}

## Session and packet logging
{: #session-and-packet-logging}

The VRA supports two types of logging, session and per packet.

### Session logging
{: #session-logging}

Use the command `security firewall session-log` to configure firewall session logging.

For UDP, ICMP, and all non-TCP flows, your session will transition to four states over the lifetime of the flow. For each transition, you can configure the VRA to log a message. TCP has a larger number of state transitions, each of which can be configured to log. The following example details the configuration of `session-log` for your firewall:

   ```sh
   set security firewall session-log icmp established
   set security firewall session-log tcp established
   set security firewall session-log udp established
   ```
   {: pre}
   
## Per packet logging
{: #per-packet-logging}

For per packet logging, ensure that you include the keyword `log` in the firewall or NAT rule. This will log every network packet that matches the rule.

Per-packet logging occurs in the packet forwarding paths and generates large amounts of output. Per packet logging can greatly reduce the throughput of the VRA, cause performance issues, and dramatically increase the disk space used for the log files. It is recommended that you use per packet logging only for debugging purposes. For all operational purposes, stateful session logging should be used instead.
{: important}
	
## Troubleshooting using firewall logs
{: #Troubleshooting-using-firewall-logs}
	
For debugging purposes, you can set up default log and per packet logging. Per packet logging can be added to each individual rule to show in the logs when packets are dropped or accepted (depending on the action set for the rule). The default log records an "implicit" drop when it happens. For the following zone-policy based firewall configuration, the default-log setting will log every time that traffic does not match rule 1. This is the only rule configured.

```sh
set security firewall name To-Private-Servers rule 1 action drop
set security firewall name To-Private-Servers rule 1 source address ServiceNetwork
set security firewall name To-Private-Servers rule 1 log
set security firewall name To-Private-Servers default-log
```
{: pre}

To view the log, there are two commands, and the following output demonstrates a logged block of specific traffic:

```sh
journalctl -u vyatta-dataplane | grep <IP Address>
show log firewall name To-Private-Servers | grep <IP Address>

vyatta@gateway02# journalctl -f -u vyatta-dataplane | grep 10.3.84.106
Feb 18 05:47:09 gateway02 vyatta-dataplane.service dataplane[4313]: fw rule To-Private-Servers:10000 block icmp(1) src=//10.3.84.106 dst=dp0bond0.1750//10.126.19.174 len=84 ttl=55 type=8 code=0
Feb 18 05:47:10 gateway02 vyatta-dataplane.service dataplane[4313]: fw rule To-Private-Servers:10000 block icmp(1) src=//10.3.84.106 dst=dp0bond0.1750//10.126.19.174 len=84 ttl=55 type=8 code=0
^C
[edit]
```
{: pre}
