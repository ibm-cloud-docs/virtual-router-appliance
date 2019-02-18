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

# Working with High Availability and VRRP
The Virtual Router Appliance (VRA) supports Virtual Router Redundancy Protocol (VRRP) as a high availability protocol. The deployment of devices is done in an active/passive manner, where one machine is master and the other is the backup. All interfaces on both machines will be a member of the same "sync-group", so if one interface experiences a fault, the other interfaces in the same group will also fault, and the device will stop being master. The current backup will detect that the master is no longer broadcasting keepalive/heartbeat messages, and assume control of the VRRP virtual IPs and become master.

VRRP is the most important part of the configuration when provisioning Gateways. High availability functionality depends on the heartbeat messages, so making sure they are not blocked is critical.

## VRRP virtual IP (VIP) addresses

The VRRP virtual IP, or VIP, is the floating IP address changed from master to backup device when failover happens. When a VRA deploys, it will have a public and private bonded network connection and real IPs assigned on each interface. A VIP is assigned on both interfaces as well, whether or not the device is a standalone or in an HA pair. Traffic that has a destination IP in subnets in VLANs associated with the VRA will be sent directly to these VRRP VIPs.

VRRP virtual IP addresses for any gateway group should not be changed, nor should the VRRP interface be disabled. These IP addresses are the method by which traffic is routed to the gateway when a VLAN is associated. If the IP address is not present, then the traffic cannot be forwarded from softLayer BCR/FCR to the gateway itself.

## VRRP group

A VRRP group consists of a cluster of interfaces or virtual interfaces that provide redundancy for a primary, or “master,” interface in the group. Each interface in the group is typically on a separate router. Redundancy is managed by the VRRP process on each system. The VRRP group has a unique numeric identifier and can be assigned up to 20 virtual IP addresses.  

The VRRP group ID is assigned by SoftLayer, and should not be changed. When a new gateway group is provisioned behind a Front Customer Router (FCR)/Backend Customer Router (BCR) for the first time, it will receive a VRRP group of 1. Subsequent gateway group provisions will increment this value to prevent conflicts, the next group will have group 2, then group 3, and so on. It is then calculated and assigned by the SoftLayer provisioning process. Altering this value risks collision with other active groups, and then master/master contention, which will likely cause an outage on both gateway groups.

If you migrate from a previous configuration, it is recommended that you double check your configuration code to make sure the VRRP group ID is not statically assigned.

The VRRP group ID is persisted in the SoftLayer database, so the same group ID value will be used during an OS reload or upgrade. Any user modified VRRP group ID will be overwritten with the system assigned value during an OS reload.  

## VRRP priority

The first machine in a gateway group will have a priority of 254, and this value is decremented for the next device provisioned. You should never set a priority to 255, as this defines the VIP "address owner" and can result in unintended behavior when the machine is brought online with a configuration that differs from the running active master.

## VRRP preemption

Preemption should always be disabled on all VRRP interfaces, so that a new device or one that is going through the reloading of its OS does not try to take over the cluster.

## VRRP advertise interval

To signal that it is still in service, the master interface or VIF sends “heartbeat” packets called “advertisements” to the LAN segment, using the IANA assigned multicast addresses for the VRRP (`224.0.0.18` for IPv4 and `FF02:0:0:0:0:0:0:12` for IPv6).

By default, the interval is set as 1. This value can be increased, but it is not recommended you set a value over 5. On a busy network, it may take much longer than a second for all the VRRP notices to arrive on the backup device from the master, so a value of 2 should be used for high traffic networks.

## VRRP synchronization (sync-group)

Interfaces in a VRRP synchronization group (“sync group”) are synchronized such that, if one of the interfaces in the group fails over to backup, all interfaces in the group fail over to backup. For example, in many cases, if one interface on a master router fails, the whole router fails over to a backup router.

This value is different than the VRRP group, as it defines what interfaces on a device will fail over when an interface in that group registers a fault. It is recommended that all interfaces belong to the same sync-group, otherwise, some interfaces will be master and have gateway IPs, and others will be backup, and traffic will not forward properly anymore. Active/active configurations are not supported.

## VRRP rfc-compatibility

rfc-compatibility enables or disables RFC 3768 MAC addresses for VRRP on an interface. This should be enabled on the native interfaces, but not enabled on any configured VIFs. Some virtual switches (mostly vmware) have problems with this being enabled, and will cause traffic to be dropped and not sent to the gateway IP from the hypervisor host machine. Leave this setting alone, and do not configure the setting for any VIFs.

## VRRP authentication
If a password is set for VRRP authentication, the authentication type must also be defined. If the password is set and the authentication type is not defined, the system will generate an error when you try to commit the configuration.

Similarly, you cannot delete the VRRP password without also deleting the VRRP authentication type. If you do, the system generates an error when you try to commit the configuration. If you delete both the VRRP authentication password and authentication type, VRRP authentication is disabled.

The IETF decided that authentication is not to be used for VRRP version 3. For more information, refer to RFC 5798 VRRP.

## Version 2/3 support
The VRA supports both VRRP version 2 (default) and version 3 protocols. In version 2, you cannot have IPv4 and IPv6 together. But in Version 3, you can have IPv4 and IPv6 at the same time.

## High Availability VPN with VRRP
The VRA provides the ability to maintain connectivity through one IPsec tunnel by using a pair of Virtual Router Appliances with VRRP. When one router fails or is brought down for maintenance, the new VRRP master router restores IPsec connectivity between the local and remote networks.

When configuring High Availability VPN with VRRP, whenever a VRRP virtual address is added to a VRA interface, you must reinitialize the IPsec daemon because the IPsec service listens only for connections to the addresses that are present on the VRA when the Internet Key Exchange (IKE) service daemon is initialized.

Run the following command on the master and backup routers, so that when failover happens, IPsec daemons will be restarted on a new master device after VIP transiting in., as shown in the following example:

`vyatta@vrouter# set interfaces bonding dp0bond1 vrrp vrrp-group 1 notify ipsec`

## High Availability Firewalls with VRRP

When two devices are in an HA pair, care must be taken on your master device that you do not block access from the other device. Port 443 must be allowed between both devices for config-sync to work, and VRRP must be allowed to be sent and received, including the multicast range of 224.0.0.0/24.

## Associated VLAN subnets with VRRP

For any VIF configuration with VRRP you will need a virtual IP address (the first usable IP in the subnet, the gateway IP to which everything in that subnet will route) and also a real interface IP address for the VIF on both devices. To conserve usable IPs, it is recommended that the real interface IPs use an out of band range such as 192.168.0.0/30, where one device has the .1 and the other device has the .2 address. You can have multiple VRRP virtual IPs, but you only need one real address on each vif.

Here is an example of a VRRP configuration with two private vlans and three subnets:

```
set interfaces bonding dp0bond0 address '10.100.11.39/26'
set interfaces bonding dp0bond0 mode 'lacp'
set interfaces bonding dp0bond0 vif 10 address '192.168.255.81/30'
set interfaces bonding dp0bond0 vif 10 description 'VLAN 10 - Two private subnets/VIPs'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 advertise-interval '1'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 preempt 'false'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 priority '254'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 sync-group 'SYNC1'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 virtual-address '10.100.105.177/28'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 10 virtual-address '10.100.38.1/26'
set interfaces bonding dp0bond0 vif 11 address '192.168.255.89/30'
set interfaces bonding dp0bond0 vif 11 description 'VLAN 11 - One private subnet/VIP'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 advertise-interval '1'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 preempt 'false'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 priority '254'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 sync-group 'SYNC1'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 11 virtual-address '10.100.212.65/26'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 preempt 'false'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 priority '254'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 'rfc-compatibility'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 sync-group 'vgroup1'
set interfaces bonding dp0bond0 vrrp vrrp-group 1 virtual-address '10.100.11.5/26'
set interfaces bonding dp0bond1 address '169.110.20.28/29'
set interfaces bonding dp0bond1 mode 'lacp'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 notify 'ipsec'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 preempt 'false'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 priority '254'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 'rfc-compatibility'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 sync-group 'SYNC1'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 virtual-address '169.110.21.26/29'
```

* A vrrp sync-group is different than a vrrp group. When an interface that belongs in a sync-group changes state, all other members of the same sync-group will transition to the same state.
* The vrrp-group number of the VLAN interfaces (VIFs) do not have to be the same as one of the native interfaces, or of the other vlans. However, it is strongly suggested to keep all virtual addresses of the same vlan in one vrrp-group, as seen in VLAN 10.
* The real interface addresses on the native vlans (e.g. dp0bond1: 169.110.20.28/29) are not always in the same subnet as their VIPs (169.110.21.26/29).

## Manual VRRP failover
If you need to force a vrrp failover, it can be achieved by running the following on the device that currently is VRRP master:

`vyatta@vrouter$ reset vrrp master interface dp0bond0 group 1`

The group ID is the vrrp group ID of the native interfaces, and, as mentioned above, could be different in your pair.

## Connection synchronization
When two VRA devices are in an HA pair, it may be useful to track stateful connections between the two devices so that if a failover happens, the current state of all connections that are on the failed device are replicated to the backup device, so that it isn't necessary for any current active sessions (like SSL connections) to be rebuilt from scratch, which can result in a disrupted user experience.

This is called connection tracking synchronization.

To configure it, you must declare what the failover method is, which interface you will use to send the connection tracking information, and the IP of the remote peer:

```
set service connsync failover-mechanism vrrp sync-group SYNC1
set service connsync interface dp0bond0
set service connsync remote-peer 10.124.10.4
```

The other VRA will have the same configuration, but a different `remote-peer`.

Note that this can saturate a link if there is a high number of connections coming in on other interfaces, and will compete with other traffic on the declared link.

## VRRP Start Delay Feature
Vyatta OS version 1801p and greater includes a new `vrrp` command.

`vrrp` specifies an election protocol that dynamically assigns responsibility for a virtual router to one of the VRRP routers on a LAN. The VRRP router controlling the IPv4 or IPv6 address(es) associated with a virtual router is called the Master, and it forwards packets sent to these IPv4 or IPv6 addresses. The election process provides dynamic failover in the forwarding responsibility should the Master become unavailable. All protocol messaging is performed using either IPv4 or IPv6 multicast datagrams; as a result, the protocol can operate over a variety of multiaccess LAN technologies supporting IPv4/IPv6 multicast.

To minimize network traffic, only the Master for each virtual router sends periodic VRRP Advertisement messages. A Backup router will not attempt to preempt the Master unless it has higher priority. This eliminates service disruption unless a more preferred path becomes available. It’s also possible to administratively prohibit all preemption attempts. If the Master becomes unavailable, then the highest-priority Backup will transition to Master after a short delay, providing a controlled transition of the virtual router responsibility with minimal service interruption.

**NOTE:** In IBM© Cloud provisioned deployments the start delay value is set to the default value. You may wish to alter this at your discretion as you test your failover and high availability methods.


### Preemption vs. No Preemption

The `vrrp` protocol defines logic that decides which VRRP peer on a network has the higher priority, and as such , the best peer to perform the role as Master. With a default configuration, VRRP will be enabled to perform preemption, which means that a new higher priority peer on the network will force failover of the Master role.

When preemption is disabled, a higher priority peer will only failover the Master role if the existing lower priority peer is no longer available on the network. Disabling preemption is sometimes useful in real world scenarios, as it copes better with situations where the higher priority peer may have started to periodically flap due to reliability issues with the peer itself or one of its network connections. It is also useful to prevent the premature failover to a new higher priority peer which has not completed network convergence.

### Assumptions and limitations of preemption

If the VRRP peers have been configured to disable preemption, then there are some cases where preemption may “appear” to occur, but in reality the scenario is just a standard VRRP failover. As described above, VRRP makes use of IP multicast datagrams as a means to confirm availability of the currently elected Master router. Since it is a layer 3 protocol that is detecting failure of a VRRP peer, it is important that failover detection in VRRP is delayed until VRRP and the layer 1 through 2 of the network stack is ready and converged. In some cases the network interface running VRRP may confirm to the protocol that the interface is up, but other underlying services like Spanning Tree, or Bonding may not have completed. As a result, IP connectivity between peers cannot be established. If this occurs, VRRP on a new higher peer will become Master, as it is unable to detect VRRP messages from the current Master peer on the network. After convergence, the brief period of time when there is two Master VRRP peers will result in the dual Master logic of VRRP being executed, the higher priority peer will remain Master, and the lower priority becoming Backup. This scenario may “appear” to demonstrate a failure in the “no preemption” functionality.

### Start Delay Feature

In order to accommodate the issues associated with delay in convergence of the lower levels of the network stack during an interface up event, as well as other contributory factors, a new feature called “Startup Delay” is introduced in the 1801p patch. The feature causes the VRRP state on a machine that has been “reloaded” to remain in the INIT state until after a predefined delay, which can be configured by the network operator. Flexibility in this delay value allows the network operator to customize the characteristics of their network and devices under real world conditions.

### Command details

```
vrrp {
start-delay <0-600>
}
```

`start-delay` can have a value between 0 (default) and 600 seconds.

### Example Configuration

```
interfaces {
  bonding dp0bond1 {
address 161.202.101.206/29 mode balanced
vrrp {
      start-delay 120
      vrrp-group 211 {
preempt false
priority 253
virtual-address 161.202.101.204/29
} }
}
```