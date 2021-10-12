---

copyright:
  years: 2017, 2019
lastupdated: "2019-11-14"

keywords:  

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

# Working with High Availability (HA) and VRRP
{: #working-with-high-availability-and-vrrp}

{{site.data.keyword.vra_full}} (VRA) supports Virtual Router Redundancy Protocol (VRRP) as a high availability protocol. The deployment of devices is done in an active/passive manner, where one machine is master and the other is the backup. All interfaces on both machines are a member of the same "sync-group", so if one interface experiences a fault, the other interfaces in the same group also fault, and the device stops being master. The current backup detects that the master is no longer broadcasting keepalive/heartbeat messages, assumes control of the VRRP virtual IPs, and becomes master.
{: shortdesc}

VRRP is the most important part of the configuration when provisioning gateways. High availability functionality depends on the heartbeat messages, so making sure that they are not blocked is critical.

## VRRP Virtual IP (VIP) addresses
{: #vrrp-virtual-ip-vip-addresses}

The VRRP virtual IP for `dp0bond1` or `dp0bond0`, or VIP, is the floating IP address that changes from master to backup device when failover happens. When a VRA deploys, it has a public and private bonded network connection and real IPs assigned on each interface. A VIP is assigned on both interfaces as well, whether the device is stand-alone, or in an HA pair. Traffic that has a destination IP in subnets in VLANs associated with the VRA is sent directly to these VRRP VIP's through a static route on the FCR/BCR.

You should neither change VRRP virtual IP addresses for any gateway group nor should you disable the VRRP interface. These IP addresses are the method by which traffic is routed to the gateway when a VLAN is associated. As a result, if they are down, VLAN traffic is also down. If the IP address is not present, then the traffic cannot be forwarded from the BCR/FCR to the VRA itself. This virtual-address or VIP is not changeable at this time. This limitation might change in the future, but currently neither the migration of a VRA between PODs/FCRs/BCRs nor changing the VIP are supported.

The following is an example of the default configurations of the `dp0bond0` and `dp0bond1` VIPs for a specific VRA. Note that your IP addresses and `vrrp-groups` might be different than this example.

```sh
set interfaces bonding dp0bond0 vrrp vrrp-group 2 virtual-address '10.127.170.1/26'
set interfaces bonding dp0bond1 vrrp vrrp-group 2 virtual-address '159.8.98.209/29'
```

For more information, see [Associated VLAN subnets with VRRP](/docs/virtual-router-appliance?topic=virtual-router-appliance-working-with-high-availability-and-vrrp#associated-vlan-subnets-with-vrrp).

**By default, VRRP is set to disabled.** This ensures that new provisions and reloads do not cause outages on the Master device. For VLAN traffic to work, VRRP must be re-enabled after provisioning or a reload completes. The following example details the default configuration:

```sh
set interfaces bonding dp0bond0 vrrp vrrp-group 2 'disable'
set interfaces bonding dp0bond1 vrrp vrrp-group 2 'disable'
```

To enable VRRP on these two interfaces after a provision or reload, **which is necessary for both stand-alone and HA pairs**, run the following commands. Then, commit the change:

```sh
delete interfaces bonding dp0bond0 vrrp vrrp-group 2 'disable'
delete interfaces bonding dp0bond1 vrrp vrrp-group 2 'disable'
```

## VRRP group
{: #vrrp-group}

A VRRP group consists of a cluster of interfaces or virtual interfaces that provide redundancy for a primary (or master) interface in the group. The VRRP group defines the `virtual_router_id` in the Keepalive configuration. In the VRRP protocol itself, it is referred to as the VRID. The VRRP group has a unique numeric identifier and can be assigned up to 20 virtual IP addresses.  

The VRRP group ID is assigned by IBM Cloud and should not be changed. When a new gateway group is provisioned behind a Frontend Customer Router (FCR)/Backend Customer Router (BCR) for the first time, it receives a VRRP group of 1. Subsequent gateway group provisions increment this value to prevent conflicts. For example, the next group has group 2, then group 3, and so on. It is then calculated and assigned by the provisioning process. Altering this value risks collision with other active groups, and then master/master contention, which likely causes an outage on both gateway groups.

If you migrate from a previous configuration, it is recommended that you double check your configuration code to make sure the VRRP group ID is not statically assigned.

The VRRP group ID is persisted in the database, so the same group ID value is used during an OS reload or upgrade. Any user modified VRRP group ID is overwritten with the system assigned value during an OS reload.  

## VRRP priority
{: #vrrp-priority}

The first machine in a gateway group has a priority of 254, and this value is decremented for the next device provisioned. You should never set a priority to 255, as this defines the VIP "address owner" and can result in unintended behavior when the machine is brought online with a configuration that differs from the running active master.

## VRRP preemption
{: #vrrp-preemption}

Preemption should always be disabled on all VRRP interfaces, so that a new device or one that is going through the reloading of its OS does not try to take over the cluster.

## VRRP advertise interval
{: #vrrp-advertise-interval}

To signal that it is still in service, the master interface or VIF sends “heartbeat” packets called “advertisements” to the LAN segment, using the IANA assigned multicast addresses for the VRRP (`224.0.0.18` for IPv4 and `FF02:0:0:0:0:0:0:12` for IPv6).

By default, the interval is set to 1. This value can be increased, but it is not recommended to set a value over 5. On a busy network, it can take much longer than a second for all the VRRP notices to arrive on the backup device from the master, so a value of 2 should be used for high-traffic networks.

## VRRP synchronization (sync-group)
{: #vrrp-synchronization-sync-group-}

Interfaces in a VRRP synchronization group (“sync group”) are synchronized such that, if one of the interfaces in the group fails over to backup, all interfaces in the group fail over to backup. For example, if one interface on a master router fails, the whole router fails over to a backup router.

This value is different than the VRRP group, as it defines what interfaces on a device fail over when an interface in that group registers a fault. It is recommended that all interfaces belong to the same sync-group; otherwise, some interfaces are master and have gateway IPs, and others are backup, and traffic does not forward properly anymore. Active/active configurations are not supported.

## VRRP rfc-compatibility
{: #vrrp-rfc-compatibility}

rfc-compatibility enables or disables RFC 3768 MAC addresses for VRRP on an interface. This should be enabled on the native interfaces, but not enabled on any configured VIFs. Some virtual switches (mostly vmware) have problems with this being enabled, and cause traffic to be dropped and not sent to the gateway IP from the hypervisor host machine. Leave this setting alone, and do not configure the setting for any VIFs.

## VRRP authentication
{: #vrrp-authenticaton}

If a password is set for VRRP authentication, the authentication type must also be defined. If the password is set and the authentication type is not defined, the system generates an error when you try to commit the configuration.

Similarly, you cannot delete the VRRP password without also deleting the VRRP authentication type. If you do, the system generates an error when you try to commit the configuration. If you delete both the VRRP authentication password and authentication type, VRRP authentication is disabled.

The IETF decided that authentication is not to be used for VRRP version 3. For more information, see RFC 5798 VRRP.

## Version 2/3 support
{: #version-2-3-support}

The VRA supports both VRRP version 2 (default) and version 3 protocols. In version 2, you cannot have IPv4 and IPv6 together; however, in Version 3, you can have IPv4 and IPv6 at the same time.

## High availability VPN with VRRP
{: #high-availability-vpn-with-vrrp}

The VRA provides the ability to maintain connectivity through one IPsec tunnel by using a pair of {{site.data.keyword.vra_full}}s with VRRP. When one router fails, or is brought down for maintenance, the new VRRP master router restores IPsec connectivity between the local and remote networks.

When configuring an HA VPN with VRRP, whenever a VRRP virtual address is added to a VRA interface, you must reinitialize the IPsec daemon because the IPsec service listens only for connections to the addresses that are present on the VRA when the Internet Key Exchange (IKE) service daemon is initialized.

Run the following command on the master and backup routers, so that when failover happens, IPsec daemons are restarted on a new master device after VIP transiting in, as shown in the following example:

`vyatta@vrouter# set interfaces bonding dp0bond1 vrrp vrrp-group 1 notify ipsec`

## High availability firewalls with VRRP
{: #high-availability-firewalls-with-vrrp}

When two devices are in an HA pair, care must be taken on your master device that you do not block access from the other device. Port 443 must be allowed between both devices for config-sync to work, and VRRP must be allowed to be sent and received, including the multicast range of `224.0.0.0/24`.

## Associated VLAN subnets with VRRP
{: #associated-vlan-subnets-with-vrrp}

For any VIF configuration with VRRP, you require a virtual IP address (the first usable IP in the subnet, the gateway IP to which everything in that subnet will route) and also a real interface IP address for the VIF on both devices. To conserve usable IPs, it is recommended that the real interface IPs use an out-of-band range, such as `192.168.0.0/30`, where one device has the .1 and the other device has the .2 address. You can have multiple VRRP virtual IPs, but you need only one real address on each VIF.

Here is an example of a VRRP configuration with two private VLANs and three subnets:

```sh
set interfaces bonding dp0bond0 address '10.100.11.39/26'
set interfaces bonding dp0bond0 mode 'lacp'
set interfaces bonding dp0bond0 vif 10 address '192.168.255.81/30'
set interfaces bonding dp0bond0 vif 10 description 'VLAN 10 - Two private subnets/VIPs'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 1 advertise-interval '1'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 1 preempt 'false'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 1 priority '254'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 1 sync-group 'vgroup1'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 1 virtual-address '10.100.105.177/28'
set interfaces bonding dp0bond0 vif 10 vrrp vrrp-group 1 virtual-address '10.100.38.1/26'
set interfaces bonding dp0bond0 vif 11 address '192.168.255.89/30'
set interfaces bonding dp0bond0 vif 11 description 'VLAN 11 - One private subnet/VIP'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 1 advertise-interval '1'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 1 preempt 'false'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 1 priority '254'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 1 sync-group 'vgroup1'
set interfaces bonding dp0bond0 vif 11 vrrp vrrp-group 1 virtual-address '10.100.212.65/26'
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
set interfaces bonding dp0bond1 vrrp vrrp-group 1 sync-group 'vgroup1'
set interfaces bonding dp0bond1 vrrp vrrp-group 1 virtual-address '169.110.21.26/29'
```

* A VRRP sync-group is different than a VRRP group. When an interface that belongs in a sync-group changes state, all other members of the same sync-group will transition to the same state.
*  The vrrp-group number of the VLAN interfaces (VIFs) should almost always be the same as its matching native interface. For example, `dp0bond1.789` should always have the same vrrp-group number as `dp0bond1`, unless the two interfaces share the same sync-group. Having a different group number on the VIF and the native interface may cause a "split brain" condition when paired with different sync groups. When two interfaces share the same sync-group, even though they are on different vrrp-groups, they will both transition between master and backup at the same time, preventing split brain. On the other hand, if the native interface is set up with a different vrrp-group number and a different sync-group as a VLAN interface, since subnets set up on VLAN interfaces are statically routed to the virtual-address on the native interface, if the VLAN interface is showing as backup and the native interface is master, the router will still send VLAN interface traffic to the VRA where the native interface is master even though the VLAN interface is secondary and not active on the VRA. This specific situation causes "split brain" and an outage. This is why it is important to be conscious of how sync-groups are set up in conjunction with vrrp-groups. It is also important to not use the same vrrp-group numbers between multiple HA VRA pairs in the same transit VLAN, since four Vyattas using the same group cause sthree VRAs to potentially assume the Backup state, while only one is Master, causing an outage.
* The real interface addresses on the native vlans (e.g. dp0bond1: 169.110.20.28/29) are not always in the same subnet as their VIPs (169.110.21.26/29).


## Manual VRRP failover
{: #manual-vrrp-failover}

If you need to force a VRRP failover, it can be achieved by running the following on the device that currently is VRRP master:

`vyatta@vrouter$ reset vrrp master interface dp0bond0 group 1`

The group ID is the VRRP group ID of the native interfaces, and, as mentioned above, could be different in your pair.

## Connection synchronization
{: #connection-synchronization}

When two VRA devices are in an HA pair, it might be useful to track stateful connections between the two devices so that if a failover happens, the current state of all connections that are on the failed device are replicated to the backup device. It isn't necessary for any current active sessions (like SSL connections) to be rebuilt from scratch, which can result in a disrupted user experience.

This is called connection tracking synchronization.

To configure it, you must declare what the failover method is, which interface you use to send the connection tracking information, and the IP of the remote peer:

```sh
set service connsync failover-mechanism vrrp sync-group SYNC1
set service connsync interface dp0bond0
set service connsync remote-peer 10.124.10.4
```

The other VRA has the same configuration, but a different `remote-peer`.

Note that this can saturate a link if there is a high number of connections coming in on other interfaces, and competes with other traffic on the declared link.

## VRRP Start Delay feature
{: #vrrp-start-delay-feature}

Vyatta OS version 1801p and greater includes a new `vrrp` command.

`vrrp` specifies an election protocol that dynamically assigns responsibility for a virtual router to one of the VRRP routers on a LAN. The VRRP router controlling the IPv4 or IPv6 addresses associated with a virtual router is called the Master, and it forwards packets that are sent to these IPv4 or IPv6 addresses. The election process provides dynamic failover in the forwarding responsibility should the Master become unavailable. All protocol messaging is performed using either IPv4 or IPv6 multicast datagrams; as a result, the protocol can operate over a variety of multiaccess LAN technologies supporting IPv4/IPv6 multicast.

To minimize network traffic, only the Master for each virtual router sends periodic VRRP advertisement messages. A backup router does not attempt to preempt the Master unless it has higher priority. This eliminates service disruption unless a more preferred path becomes available. It’s also possible to administratively prohibit all preemption attempts. If the Master becomes unavailable, then the highest-priority backup transitions to Master after a short delay, providing a controlled transition of the virtual router responsibility with minimal service interruption.

In IBM Cloud provisioned deployments, the start delay value is set to the default value. You might want to alter this at your discretion as you test your failover and high availability methods.
{: note}

### Preemption versus no preemption
{: #preemption-vs-no-preemption}

The `vrrp` protocol defines logic that decides which VRRP peer on a network has the higher priority, and as such, the best peer to perform the role as Master. With a default configuration, VRRP is enabled to perform preemption, which means that a new higher priority peer on the network forces failover of the Master role.

When preemption is disabled, a higher priority peer will only failover the Master role if the existing lower priority peer is no longer available on the network. Disabling preemption is sometimes useful in real world scenarios, as it copes better with situations where the higher priority peer might have started to periodically flap due to reliability issues with the peer itself or one of its network connections. It is also useful to prevent the premature failover to a new higher priority peer which has not completed network convergence.

### Assumptions and limitations of preemption
{: #assumptions-and-limitations-of-preemption}

If the VRRP peers are configured to disable preemption, then there are some cases where preemption may “appear” to occur, but in reality the scenario is just a standard VRRP failover. As described above, VRRP makes use of IP multicast datagrams as a means to confirm availability of the currently elected Master router. Since it is a layer 3 protocol that is detecting failure of a VRRP peer, it is important that failover detection in VRRP is delayed until VRRP and the layer 1 through 2 of the network stack is ready and converged. In some cases, the network interface running VRRP might confirm to the protocol that the interface is up, but other underlying services like Spanning Tree, or Bonding might not have completed. As a result, IP connectivity between peers cannot be established. If this occurs, VRRP on a new higher peer becomes Master as it is unable to detect VRRP messages from the current Master peer on the network. After convergence, the brief period of time when there is two Master VRRP peers results in the dual Master logic of VRRP being executed. The higher priority peer remains Master, and the lower priority becomes  backup. This scenario might appear to demonstrate a failure in the “no preemption” functionality.

### Start Delay feature
{: #start-delay-feature}

To accommodate the issues associated with delay in convergence of the lower levels of the network stack during an interface up event, as well as other contributory factors, a new feature called “Startup Delay” is introduced in the 1801p patch. This feature causes the VRRP state on a machine that was “reloaded” to remain in the INIT state until after a predefined delay, which can be configured by the network operator. Flexibility in this delay value allows the network operator to customize the characteristics of their network and devices under real world conditions.

### Command details
{: #command-details}

```sh
vrrp {
start-delay <0-600>
}
```

`start-delay` can have a value between 0 (default) and 600 seconds.

### Example configuration
{: #example-configuration}

```sh
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
